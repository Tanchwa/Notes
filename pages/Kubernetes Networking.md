- [[Linux Networking]] review
	- `ip link` displays the eth physical devices
	- `ip addr add 192.168.1.11/24 dev eth0`
	- `ip route` displays route table
	- `ip route add 192.168.2.0/24 via 192.168.1.1`
	- `ss` show sockets on the host, some good options:
		- -l --listening show listening, not shown by default
		- -p --program show the PID of the program to which the socket belongs
		- -n --numeric show numerical addresses instead of trying to determine symbolic host, port, or usernames
	- advanced DNS settings
		- entering "search" in resolve.conf can help you specify multiple domains you want to search similar names
			- ```
			  nameserver		8.8.8.8
			  search			mycompany.com can.add up.to six.here
			  ```
			- this will append .mycompany.com to any unresolved queries. for example `ping web` command will resolve to web.mycompany.com
- Linux Network Namespaces
  id:: 62fac7da-9c9e-48fb-80d6-3a9c31d03753
	- namespaces are run on the linux host
	- [[Docker]] takes advantage of these to separate container networks
	- run the `ip netns add NAME` to create a namespace
	- run the `ip netns exec NAMESPACE COMMAND` or `ip -n NAMESPACE COMMAND` to run a specific command in that namespace
	- connect two namespaces using virtual ethernet links (pipes)
		- `ip link add NAME_VETH type veth peer name OTHER_NAME_VETH`
		- then attach with `ip link set NAME_VETH netns NAMESPACE`
		- then assign an ip with `ip -n NAMESPACE addr add 192.168.15.1 dev NAME_VETH`
	- connect many using a virtual switch
		- many solutions available: linux bridge, virtual eth... etc.
		- ip link add `VETH_NAME type veth peer name VETH_NAME_BRIDGE`
		- then add the `ip link set VETH_NAME master v-net-0` v-net-0 is name of v-switch
		- if you want to reach these connected to the v-net-0, just assign an ip address to the v-net-0
		  `ip addr add 192.168.15.5/24 dev v-net-0`
	- namespace routing and connecting to external LAN   network locations
		- add an entry into the routing table of the namespace for the gateway.
			- this is a little different than normal, you have to define the Destination AND the Gateway here. `ip netns exec blue ip route add 192.168.1.0/24 via 192.168.15.5`
			- think we can still ping and get replies? HAH ANOOOO we still have to configure NAT
				- you'll use [[IP Tables]] to add a prerouting rule
				- `iptables -t nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE`
				- https://explainshell.com/explain?cmd=iptables%20-t%20nat%20-A%20POSTROUTING%20-s%20192.168.15.0/24%20-j%20MASQUERADE
			- we still won't be able to ping the internet, add a destination for 0.0.0.0/ default via 192.168.1.0
	- to ping the namespaces from outside hosts
		- giveaway the name of the node: add a route entry for the namespace in the outside node
		- or set up a port forwarding rule in [[IP Tables]] `iptables -t nat -A PREROUTING --dport 80 --to-destination 192.168.15.2:80 -j DNAT`
		- https://explainshell.com/explain?cmd=iptables+-t+nat+-A+PREROUTING+--dport+80+--to-destination+192.168.15.2%3A80+-j+DNAT
-
- in docker, it doesn't advertise/ mount/ make a sym link automatically so some hackery needs to be done to see the network namespace for the container
- see this article https://stackoverflow.com/questions/31265993/docker-networking-namespace-not-visible-in-ip-netns-list
- also see [Creating Containers by Hand](https://www.redhat.com/sysadmin/net-namespaces)
-
-
- to make a Bridge network, [[Kubernetes]], Rkt, Mesos, and all container networking solutions for that matter, have set commands that they use
	- for example, the `bridge add CONTAINER_ID NAMESPACE``bridge add CONTAINER_ID /var/run/netns/CONTAINER_ID` is basically running everything explained in ((62fac7da-9c9e-48fb-80d6-3a9c31d03753))
	- but how do we know that these different solutions will run the script correctly?
	- this is where [[CNI]] comes in
	- this "bridge" program counts as a plugin for [[CNI]]
	- [[Docker]] actually has its own version of [[CNI]], CNM.
		- If you want to use [[CNI]] with docker containers, you have to run it with no network, and then manually invoke the [[CNI]] plugins.
		- This is how [[Kubernetes]] does it
-
- Cluster Networking
  id:: 63036fc2-09b7-46b4-a3ef-3eb54054249b
	- Important Ports
		- [[Kube-API]] 6443
		- [[Kubelet]] on 10250
		- [[Kube-Scheduler]] 10251
		- [[Kube-Controller-Manager]] 10252
		- the worker nodes expose services for external access on 30000- 32767
		- [[ETCD]] is on 2379, and if you have a load ballenced [[ETCD]], you need 2380 open for the various [[ETCD]] instances to communicate
-
- Important File Locations
	- [[CNI]] Binaries are stored in /opt/cni/bin
	- the configured [[CNI]] plugin can be found at /etc/cni/net.d
		- if you look into it, you can see more information on what binaries are run when a container is created and other options for routing
-
- IP address ranges
	- [[Service]] IP range can be found in the command section of the [[kube-apiserver.yaml]] or in the [[unit file]], or check it using the ps aux | grep kube-api-server
	- [[Pod]] IP ranges can be found in the logs of your [[CNI]] pluggin container
-
- what proxy-er [[Kube-Proxy]] uses can be found in the /var/log/kube-proxy.log
	- `kubectl logs <pod name> -n kube-system`
-
- DNS in Kubernetes
	- FQDN - Hostname.Namespace.Type.Root(cluster.local): web-service.apps.svc.cluster.local
	- for pods its 10-244-2-5 (pod ip but with dashes). apps.pod.cluster.local
	-
	- DNS is run by [[CoreDNS]
		- config is stored in /etc/coredns/Corefile
		- this is passed through to the [[Pod]] using a [[Config Map]]
	-
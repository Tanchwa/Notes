tags:: Kubernetes [[Kubernetes Object]]

- Kube Proxy controls services and how they are created
- it gets the ip address of the pods and creates forwarding rules from the service
	- (its actually an IP:port combo)
- different ways these rules are created
	- userspace - kube-proxy listens on a port for each service and proxies connections to the pods
	- ipvs
	- iptables - default and sets up NAT rules to forward traffic
	- this can usually be found /var/log/kube-proxy.log inside the kube proxy container
	  `kubectl logs <pod name> -n kube-system`
-
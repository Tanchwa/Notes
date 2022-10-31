tags:: Kubernetes

- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
-
- INCOMPLETE, REWATCH VIDEO
- Pre-Installation Tasks
	- need a supported operating system (2gb of ram, 2CPUs)
	- Set up nodes, set the IPs and open the proper ports
		- let IPtables see bridged traffic: make sure the br_netfilter kernel module is loaded `lsmod | grep br_netfilter` if not there, run `modprobe br_netfilter`
		- In order for a Linux node's iptables to correctly view bridged traffic, verify that  `net.bridge.bridge-nf-call-iptables`  is set to 1 in your  `sysctl`  config. For example:
			- ```
			  cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
			  overlay
			  br_netfilter
			  EOF
			  - sudo modprobe overlay
			  sudo modprobe br_netfilter
			  - *# sysctl params required by setup, params persist across reboots*
			  cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
			  net.bridge.bridge-nf-call-iptables  = 1
			  net.bridge.bridge-nf-call-ip6tables = 1
			  net.ipv4.ip_forward                 = 1
			  EOF
			  - *# Apply sysctl params without reboot*
			  sudo sysctl --system
			  ```
		- open necessary ports, see ((63036fc2-09b7-46b4-a3ef-3eb54054249b))
	- install container runtime
	- #+BEGIN_NOTE
	  DOCKER IS DEPRECATED, YOU WILL NEED TO USE CRI-O or CONTAINERD
	  #+END_NOTE
		- Follow the instructions for [getting started with containerd](https://github.com/containerd/containerd/blob/main/docs/getting-started.md). Return to this step once you've created a valid configuration file,  `config.toml` .
		- On Linux the default CRI socket for containerd is  `/run/containerd/containerd.sock` . On Windows the default CRI endpoint is  `npipe://./pipe/containerd-containerd` .
		- To use the  `systemd`  cgroup driver in  `/etc/containerd/config.toml`  with  `runc` , set
		- ```
		  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
		  ...
		  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
		    SystemdCgroup = true
		  ```
	- Install [[Kubeadm]], Kubectl, and [[Kubelet]] on all the nodes
	- decide what our CIDR range should be for our POD network
- Installation Tasks
	- initialize the cluster and add the CIDR ranges for the pod network and the api server address
	  `kubeadmin init --pod-network-cidr=10.244.0.0/16 --api-server-adress=192.168.56.2`
	- [[Kubeadm]] proceeds with creation of certs and installation of various components
- Post-Installation Tasks
	- need to run the following as a regular user
	- create a /.kube directory under the user's home directory and copy the /etc/kubernetes/admin.conf file to /.kube/config, don't forget to chwon to the user's ID
	- install and set up your [[CNI]] Network Plugin to install a pod network
		- it will create the necessary components: service account, cluster role, role binding, daemonsets
	- add worker nodes to the cluster with the `kubeadm join IP.ADD.RESS.HERE --token 081734974927853 --discovery-token-ca-cert-hash sha256:013798479847uf9424ojoe9r`
	-
-
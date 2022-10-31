tags:: Kubernetes [[Kubernetes Object]]

-
- Important File Locations
	- the [[Kubelet]] service file is usually in /etc/systemd/system/kubelet.service.d
	- the [[Kubelet]] service takes options from /var/lib/kubelet/kubelet.conf
	- other important things you'll need that can be found in the service file are the [[Kubeconfig]], and kubelet.conf
		- kubelet.conf can show you a few important things such as
		- staticPodPath
tags:: Kubernetes [[Kubernetes Object]]

- The Kube-API component in Kubernetes is responsible for provide access to the cluster
  this can either be done through cURL requests to the [[RestAPI]], or through [[kubectl commands]]
- The Kube-API is responsible for authenticating and processing access to the cluster through username and password (not recommended), usernames and tokens (also not recommended) Certificates, or an identity service like [[LDAP]]
  :LOGBOOK:
  CLOCK: [2022-08-05 Fri 16:17:38]
  :END:
- Authenticating to Kube-API
  id:: 62ed7bab-ae32-4833-8bbd-28ddd8e6898c
	- we can authenticate to the Kube-API by specifying the client key, the cert, and the ca cert when you run [[kubectl commands]] or make a [[RestAPI]] call
	  `curl https://kube-apiserver:6443/api/v1/pods -- key admin.key --cert admin.crt --cacert ca.crt`
	- or move all these certificates into a [[Kubeconfig]] file and using that every time we make a command
	- we can also authenticate using a kubectl proxy client, which launches a proxy service locally at port 8001
	- uses certificates from [[Kubeconfig]] to access the cluster
		- `kubectl proxy'
		- `curl http://localhost:8001 -k`
-
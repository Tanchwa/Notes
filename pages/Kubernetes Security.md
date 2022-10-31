tags:: [[Kubernetes]]

-
- [[TLS]] in Kubernetes
  is done the same way in a normal PKI, but within the cluster
	- each client can be authenticated with their own certificate
	- Naming conventions:
		- Public Keys:
			- *.cert or *.pem
		- Private Keys: usually have "key" in them
			- *.key or *-key.pem
	- Server Certs:
		- [[Kube-API]]
		- [[ETCD]]
			- sometimes ETCD will have its own [[certificate authority]]
			- the [[Kube-API]] is the only client that connects to the [[ETCD]], so it can use its own server keys, or a new set that is for client access to the [[ETCD]]
		- [[Kubelet]]
			- the [[Kube-API]] also needs to talk to the [[Kubelet]], and can also use a new set of Kube-API-Client certs
	- Client Certs:
		- Admins
		- [[Kube-Scheduler]]
		- [[Kube-Controller-Manager]]
		- [[Kube-Proxy]]
	- [[Kubernetes]] requires at least one [[certificate authority]]
	  :LOGBOOK:
	  CLOCK: [2022-08-05 Fri 16:11:00]
	  :END:
- Certificate creation in Kubernetes
	- [[certificate authority]] private key can be generated with the command `openssl genrsa -out ca.key 2048`
	- then a certificate signing request needs to be made to make a new unsigned cert
	  :LOGBOOK:
	  CLOCK: [2022-08-05 Fri 16:37:38]--[2022-08-05 Fri 16:37:43] =>  00:00:05
	  :END:
	  `openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr`
	  we pass in the key that we want to be used as the public key in the certificate, and the Cname we want on the certificate
	- but then the certificate needs to be signed with the [[certificate authority]]'s own private key
	  `openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt`
	-
	- We do the same thing for client keys, but we sign them with the existing ca
	- `opensssl genrsa -out admin.key 2048`
	- `openssl req -new -key admin.key -subj "/CN=kube-admin" -out admin.csr`
	- `openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt`
	- the admin needs be able to be differentiated as an admin user, so the group details need to be added, `/O=system:masters` in the openssl signing request
-
	- Server Certificates:
		- the [[ETCD]] sometimes needs to be spread across multiple nodes, 
		  in this case peer-certificates must be made and specified in the [[unit file]] or in [[etcd.yaml]]
		- the [[Kube-API]] needs to be specified by all of its alternative names referenced by other cluster components:
		  kubernetes, kubernetes.default, kubernetes.default.svc, kubernetes.default.svc.cluster.local
		  this needs to be done by making an [[openssl.cnf]] file and specifying it under the [alt names] section
		  run the `openssl req -new -key apiserver.key -subj "/CN=kube-apiserver" -out apiserver.csr -config openssl.cnf` command to make the signing request
		  `openssl x509 -req -in apiserver.csr -CA ca.cert -CAkey ca.key -out apiserver.crt`
		- the [[Kubelet]] needs a separate cert for every node, named after the nodes
	- System Client certificates:
		- the [[Kubelet]] also needs a set of client files in order to authenticate to the [[Kube-API]]
		  these will be named system:node:nodename
		- the rest of the client certificates for the [[Kube-Scheduler]], [[Kube-Proxy]] and the [[Kube-Controller-Manager]] are prefaced with `system:`
-
-
- Authenticating to the cluster
	- see ((62ed7bab-ae32-4833-8bbd-28ddd8e6898c))
-
- Viewing certificates in Kubernetes
	- LATER rewatch and take notes on the "View Certificate Details" video
	- [kubernetes-certs-checker.xlsx](../assets/kubernetes-certs-checker_1659731044760_0.xlsx)
	-
- Creating certificates for user/ admin access
	- LATER  rewatch and take notes on the "Certificates API" video
	  :LOGBOOK:
	  CLOCK: [2022-08-05 Fri 17:39:09]
	  :END:
-
- Authorization methods:
  authorization mode can be defined in the `authorization-mode=` section of the [[unit file]] or [[kube-apiserver.yaml]] these are followed in order
	- Nodes-
	- Webhooks -
	- ABAC-
	- RBAC-
-
- RBAC and Cluster Roles
	- LATER rewatch and take notes on the two videos, RBAC and cluster roles
-
- Service accounts
  service accounts are used by machines
	- examples:
	  [[Prometheus]], [[Jenkins]], etc.
	- if you want to build an app to query the [[Kube-API]], you need to give it a service account
	- `kubectl create serviceaccount NAMEOFSERVICEACCOUNT`
	- this also creates a token automatically for the service account to authenticate
	  created as a secret object, view by running 
	  `kubectl describe secret NAMEOFSERVICEACCOUNT-token-hsdfkj`
	  the token then needs to be exported so the app can authenticate itself 
	  `curl this://is.a.url.or:something/api -insecure --header "Autherization: Bearer TOKENTOKEN"`
	- this is made a lot simpler if its running IN the cluster, just mount the [[Secret]] as a [[Volume]]
	- every [[Namespace]] has its own service account called default
	  its token is automatically mounted to any pod as a volume mount, no need to define it
	- tokens are stored in the pod in `/var/run/secrets/kubernetes.io/serviceaccount`
	- you can add a custom serviceAccountName in the [[pod-definition.yaml]]
	- or set none with automountServiceAccountToken: false
-
- Image Security
	- see: ((62edc4d7-6338-43fd-90df-62dfcc2fbf71))
	- custom image repos can be configured in the deployment by specifying `imagePullSecrets:`under the spec: section in line with the containers: config
-
- Security Context
	- See ((62efc6c3-b65e-4c5d-a040-c062a77d2684))
	- security context can be specified in the [[pod-definition.yaml]] file
	  if you want it to be defined at the pod level, add `securityContext` under the spec field in [[pod-definition.yaml]]
	  spec:
	      securityContext:
	          runAsUser: 1000 
	  if you want it on the container, move all of this under the `containers:` section
	  container defined overrides pod defined
	- you can also add `capabilities:` under the container defined section
	  containers:
		- name:
		  image:
		  securityContext:
		      runAsUser: 1000
		     capabilities:
		         add: ["CAP_NAME"]
	-
-
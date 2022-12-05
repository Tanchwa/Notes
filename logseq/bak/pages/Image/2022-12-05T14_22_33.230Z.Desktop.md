tags:: Kubernetes Object, Kubernetes, Docker

-
- image: registry/userOrAccount/imageOrRepository
-
- docker.io/ is the dns name of the default [[Docker]] registry
	- Google's is gcr.io/  This is usually where many [[Kubernetes]] related images are found
- [[Amazon EKS]], [[Azure AKS]], or [[GCP GKE]] all offer managed registries by default
-
- library/ is the name of the default account where [[Docker]] official images are kept
-
- Securing Image Repositories in Kubernetes
  id:: 62edc4d7-6338-43fd-90df-62dfcc2fbf71
	- in [[Kubernetes]], we know images are pulled and run with the [[Docker]] runtime in the worker nodes
	- private registries are authenticated in [[Kubernetes]] with a [[Secret]] object with type docker-registry, named regcred 
	  `kubectl create secret docker-registry regcred --docker-server= private-registry.io --docker-username= registry-user --docker-password= registry-password --docker-email= registry-user@org.com`
	- we then specify the secret with the term `imagePullSecrets:` under the `spec:` section in [[pod-definition.yaml]]
	-
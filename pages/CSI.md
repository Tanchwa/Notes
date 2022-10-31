- Container Storage interface
- vendor agnostic standard for different storage solutions within [[Kubernetes]] or any other container orchestration tool
  id:: 62f1560f-2f27-43f4-b79e-89a7a0c90992
-
- defines a set of RPC's or remote procedure calls that are called upon by the container orchestrator
	- these must then be implemented by the storage drives
	- ex: CSI says "when a pod is created and requires a volume, the orchestrator should call the 'create volume' RPC and pass a set of details to the storage driver"
	  id:: 62f15741-342b-448b-8943-e5c55b976e46
	- the storage drive then handles this RPC request and creates a new volume and returns the results of the operation
-
- more details can be found on the specification at https://github.com/container-storage-interface/spec
-
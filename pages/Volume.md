tags:: Kubernetes, Kubernetes Object, [[Docker]]

-
-
-
- Volume Mounts
	- volume mounts can be mounted from within the [[pod-definition.yaml]] page
	- this is the simplest way, much like docker, to configure a host path for the data
	- ```
	  **spec**:
	    **containers**:
	    - **image**: k8s.gcr.io/test-webserver
	      **name**: test-container
	      **volumeMounts**:
	      - **mountPath**: /path/inside/container
	        **name**: test-volume
	    **volumes**:
	    - **name**: test-volume
	      **hostPath**:
	        **path**:  /path/outside/container
	        **type**:  directory
	  ```
	- but this is not recommended for a multi node cluster
	- YOU CAN however, mount this to an [[NFS]]:
- ```
  **spec**:
    **containers**:
    - **image**: k8s.gcr.io/test-webserver
      **name**: test-container
      **volumeMounts**:
      - **mountPath**: /test-ebs
        **name**: test-volume
    **volumes**:
    - **name**: test-volume
      *# This AWS EBS volume must already exist.*
      **awsElasticBlockStore**:
        **volumeID**: "<volume id>"
        **fsType**: ext4
  ```
-
-
- see: [[Persistent Volume]]
- see: [[Persistent Volume Claim]]
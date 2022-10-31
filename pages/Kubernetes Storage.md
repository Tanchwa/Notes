tags:: Kubernetes

- We must first understand [[Docker Storage]]
-
- The need for brand agnosticism
	- In the past, [[Docker]] was the only container runtime, but now they need to be able to work with other container runtimes like Rkt, Cri-o, Podman, etc.
	- for this reason, the [[CRI]] was implemented
- This was further extended to different [[Kubernetes Networking]] solutions with the [[CNI]]
- Same with the [[CSI]]
	- different vendors can now create their own plugin to work with Kubernetes
-
- [[Volume]]
	- volumes can be defined the same way as [[Docker Storage]] is defined in a [[docker-compose.yaml]] file
	- in the [[pod-definition.yaml]], create a volume under spec:
	- you then need to mount it under the container specification block, using volumeMounts
		- this is the path to the inside of the container
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
- [[Persistent Volume]]
	- Volumes are defined in the pod definition, meaning that every time someone makes a pod, they need to define storage. This is super inefficient and may end up breaking things
	- Managing storage more centrally
		- a Persistent volume is a cluster wide pool of storage volumes configured by an admin to be used by users of the cluster
		- these can be claimed with a [[Persistent Volume Claim]]
		- [[pv-definition.yaml]]
		- access modes define how a volume is mounted on the host, usually defined by node amounts
			- The access modes are:
			- `ReadWriteOnce` 
			  the volume can be mounted as read-write by a single node. ReadWriteOnce access mode still can allow multiple pods to access the volume when the pods are running on the same node.
			   `ReadOnlyMany` 
			  the volume can be mounted as read-only by many nodes.
			   `ReadWriteMany` 
			  the volume can be mounted as read-write by many nodes.
			   `ReadWriteOncePod` 
			  the volume can be mounted as read-write by a single Pod. Use ReadWriteOncePod access mode if you want to ensure that only one pod across whole cluster can read that PVC or write to it. This is only supported for CSI volumes and Kubernetes version 1.22+.
- [[Persistent Volume Claim]]
	- are used by users to say "hey I want this much"
	- and then [[Kubernetes]] binds these to [[Persistent Volume]]s based on the requests and properties set on the volume
		- these binds are based on
			- capacity
			- access modes
			- volume modes
			- storage class
			- selectors if you want to mount it to a specific one
		- note, a larger [[Persistent Volume]] may be claimed by a smaller [[Persistent Volume Claim]] if all the other criteria matches, and there are no better options
		- these are 1 to 1
		- if no [[Persistent Volumes]] are available, there will be unclaimed [[Persistent Volume Claim]]s waiting until a new one is made
		- see [[pvc-definition.yaml]]
-
- if you delete a [[Persistent Volume Claim]], you can choose what happens to the underlying [[Persistent Volume]] with `persistentVolumeREclaimPolicy`
	- Retain: data will be kept (default)
	- Delete: volume is deleted
	- Recycle: the Volume stays but the data is scrubbed
-
- [[Storage Class]]
	- let you provision disk storage on the fly with different cloud provisioners or other storage solutions.
	- [[sc-definition.yaml]]
	-
-
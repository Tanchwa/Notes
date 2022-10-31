title:: pvc-definition.yaml

- ```
  apiVersion: v1
  kind: PersistentVolumeClaim
  metada:
  	name: myclaim
  spec:
  	accessModes:
      	- ReadOnlyMany #only one can be used
      	- ReadWriteOnce
          - ReadWriteMany
      resources:
      	requests:
          	storage: 500Mi
  ```
-
- Once you create a PVC use it in a [[pod-definition.yaml]] by specifying the PVC Claim name under persistentVolumeClaim section in the volumes section like this:
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: mypod
  spec:
    containers:
      - name: myfrontend
        image: nginx
        volumeMounts:
        - mountPath: "/var/www/html"
          name: mypd
    volumes:
    	- name: mypd
          persistentVolumeClaim:
          	claimName: myclaim
  ```
-
- The same is true for ReplicaSets or Deployments. Add this to the pod template section of a Deployment on ReplicaSet.
-
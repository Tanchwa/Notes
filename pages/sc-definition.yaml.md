title:: sc-definition.yaml

- ```
  **apiVersion**: storage.k8s.io/v1
  **kind**: StorageClass
  **metadata**:
    **name**: standard
  **provisioner**: kubernetes.io/aws-ebs
  **parameters**:
    **type**: gp2
  **reclaimPolicy**: Retain
  **allowVolumeExpansion**: **true**
  **mountOptions**:
    - debug
  **volumeBindingMode**: Immediate #can also be WaitForFirstCustomer
  ```
-
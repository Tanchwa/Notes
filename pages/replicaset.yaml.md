title:: replicaset.yaml

- ```
  **apiVersion**: apps/v1
  **kind**: ReplicaSet
  **metadata**:
    **name**: frontend
    **labels**:
      **app**: guestbook
      **tier**: frontend
  **spec**:
    *# modify replicas according to your case*
    **replicas**: 3
    **selector**: ##THIS IS A REQUIRED FIELD ONLY FOR REPLICA SETS, OPT FOR REP CONTROLER
  THIS IS BECAUSE REPLICASETS CAN ALSO CONTROL PODS NOT MADE WITH REPLICA SETS#
      **matchLabels**:
        **tier**: frontend
    **template**:
      **metadata**:
        **labels**:
          **tier**: frontend
      **spec**:
        **containers**:
        - **name**: php-redis
          **image**: gcr.io/google_samples/gb-frontend:v3
  ```
-
-
-
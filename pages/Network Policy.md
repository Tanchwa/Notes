tags:: [[Kubernetes Object]] [[Kubernetes]]

- can add policies to pods, similar to security groups in AWS
-
- how to link Network Policies to pods? 
  similar to how you link labels and selectors for [[Replica Set]] or [[Service]]s to pods
- inside [[network-policy.yaml]]
	- podSelector:
	      matchLabels:
	          role: db
	-
- when you associate a network policy, it blocks out all undefined traffic
	- IT WILL ALSO BLOCK INCOMING TRAFIC FROM THE API
- it is a stateful firewall, just like AWS
- if you have two settings under the same rule (ie: one dash, one array element), this would be an AND rule
- example:
- ```
  **ingress**:
      - **from**:
          - **ipBlock**:
              **cidr**: 172.17.0.0/16
              **except**:
                - 172.17.1.0/24
          - **namespaceSelector**:
              **matchLabels**:
                **project**: myproject
          - **podSelector**:
              **matchLabels**:
                **role**: frontend
  ```
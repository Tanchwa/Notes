- an ingress controller is like an agent object that runs in a namespace to grant access from an ingress object to resources in the namespace
- in that way, its kind of like [[Kube-Proxy]]
- I'm still unsure if it needs to be deployed on multiple nodes or just across a single on across a collective namespace, but so far it looks like there aren't any restrictions on that
-
- Ingress Controller is created as a deployment
- :LOGBOOK:
  CLOCK: [2022-08-24 Wed 09:15:50]--[2022-08-24 Wed 09:15:50] =>  00:00:00
  CLOCK: [2022-08-24 Wed 09:15:51]
  :END:
-
-
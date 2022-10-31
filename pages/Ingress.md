- Ingress gives people external access the cluster
- ingress acts like a traditional layer 7 load balancer/ reverse proxy that load balances [[Service]]s within the cluster
-
- Ingress needs an ingress object which defines the rules, and then an [[Ingress Controller]] which fulfills the rules.
-
- Ingress can be fulfilled by many different applications, and the styling is a little bit different for each. Nginx, HAProxy, Trafeik, and cloud providers all have their own flavors of ingress based on their own load balancers.
-
- LATER add ingress sample yaml
  :LOGBOOK:
  CLOCK: [2022-08-23 Tue 07:55:07]
  :END:
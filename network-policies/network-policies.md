## Network Policies
* `NetworkPolicy` is used to configure ingress/egress traffic rules between pods.
* NetworkPolicies support is based on the networking solution deployed in the cluster. some do not support NetworkPolicies. Example, Calico, weave-net supports whereas flannel does not.
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      app: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: api
          namespaceSelector:
            matchLabels:
              env: prod
        - ipBlock:
            cidr: 192.168.5.10/32
      ports:
        - protocol: TCP
          port: 3306
  egress:
    - to:
        - ipBlock:
            cidr: 192.168.5.10
      ports:
        - protocol: TCP
          port: 80

```
Note: In the above example there are 2 rules - Rule 1 includes podSelector and namespaceSelector. Both conditions must be satisfied to allow traffic.
Rule2 is a IP address block. Rule1 and Rule2 are combined with a 'OR' condition. 
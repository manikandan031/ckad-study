## Node Selectors and Node Affinity
* `nodeSelector` is used to enforce a Pod to be scheduled on specific Nodes (with labels)
```shell
# create Label on Node
kubectl label nodes node01 size=Large
```
```yaml
spec:
  containers:
    - name: nginx
      image: nginx
  nodeSelector:
    size: Large
```
```yaml
spec:
  containers:
    - name: nginx
      image: nginx
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: size
                operator: In
                values:
                  - Large
                  - Medium                 
```
* Node Affinity Types
  * requiredDuringSchedulingIgnoredDuringExecution
  * preferredDuringSchedulingIgnoredDuringExecution
  * requiredDuringSchedulingRequiredDuringExecution (planned)
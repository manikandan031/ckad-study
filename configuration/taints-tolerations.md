### Taints and Tolerations

* `Taints` are used to configure a Node to only accept a certain type of pods to be scheduled in it.
* `Tolerations` are configured in the pod to tolerate a taint so that it gets scheduled in such nodes with a Taint.
```yaml
spec:
  containers:
    - name: nginx
      image: nginx
      tolerations:
        - key: "app"
          operator: "Equal"
          value: "Blue"
          effect: "NoSchedule"
```
```sh
kubectl taint node Node1 app=Blue:NoSchedule
#untaint
kubectl taint node Node1 app-Blue:NoSchedule-
```
* Effect 
  * NoSchedule - New pod will not be scheduled 
  * PreferNoSchedule - New pod preferably not scheduled but no guarantee.
  * NoExecute - New pod not scheduled and existing pod if any will be evicted.
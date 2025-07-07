## Labels and selectors
```shell
kubectl get pods -l k1=v1,k2=v2,k3=v3
kubectl get pods -l 'k1 in (v1,v2)'
```
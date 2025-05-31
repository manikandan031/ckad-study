## Namespace
* kubernetes create the `default` namespace by default. Other k8 namespaces are `kube-system` , `kube-public`
* commands
```
kubectl get pods --namespace=finance

kubectl config set-context $(kubectl config current-context) --namespace=dev
```
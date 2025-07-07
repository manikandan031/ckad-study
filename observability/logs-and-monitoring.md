## Logs & Monitoring
```shell
# tail logs of a specific container of a pod
kubectl logs -f my-pod my-container 

# search logs
kubectl logs my-pod my-container | grep <search-string>
```
* By default, kubernetes comes with minimal monitoring. Run `Metrics Server` for a simple in-memory monitoring of nodes and pods for resource utilization i.e. cpu, memory, disk.
* For persistent and advanced monitoring use tools such as - prometheus, Elastic stack, Data Dog, Dynatrace
* Deploy Metric Server from `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`
* `kubelet` has a component `cAdvisor` which collects metrics from pods and exposes metrics via kubelet apis to the metrics server.
```shell
# Deploy Metrics Server
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
# Node metrics
kubectl top node
# Pod metrics
kubectl top pod
```
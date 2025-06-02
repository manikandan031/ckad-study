## Imperative commands
```bash
# create pod
kubectl run nginx --image=nginx

#  quick create a pod definition yml instead of writing from scratch
kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yml
 
# create deployment
kubectl create deployment nginx --image=nginx
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yml

# create deployment with replicas
kubectl create deployment nginx --image=nginx --replicas=4

# create service (automatically uses pods labels as selectors)
kubectl expose pod redis-pod --port=6379 --name=redis-service 

# create pod and expose as service
kubectl run nginx-pod --image=nginx:latest --port=80 --expose=true 

#help
kubectl create service --help
kubectl create service clusterip --help
kubectl expose --help

```
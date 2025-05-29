* useful commands
```
# create pod
kubectl run nginx --image=nginx

#  quick create a pod definition yml instead of writing from scratch
kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx-pod.yml
 
# create deployment
kubectl create deployment nginx --image=nginx
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml > nginx-deployment.yml

# create deployment with replicas
kubectl create deployment nginx --image=nginx --replicas=4

# create service
kubectl expose pod redis-pod --port=6379 --name=redis-service (this automatically uses pods labels as selectors)

```
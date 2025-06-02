### Config Maps
* Environment variables for pods
```
spec:
  containers:
   - name: nginx
     image: nginx
     env:
       - name: APP_COLOR
         value: green
```
* ConfigMaps are used to remove hardcoded environment variables in the pod definition and manage them centrally
```
# create config map - imperative - from-literal
kubectl create configmap myapp-config --from-literal=APP_COLOR=green --from-literal=APP_VERSION=1.0

# create config map - imperative - from-file
kubectl create configmap myapp-config --from-file my-app.properties

# view config map data
kubectl describe configmap myapp-config
```

* inject env variable from configmap in a pod - all configs
```
spec:
  containers:
   - name: nginx
     image: nginx
     envFrom:
      - configMapRef:
          name: myapp-cnfig
```

* inject env variabe from configmap in a pod - specific config from configmap
```
spec:
  containers:
    - name: nginx
      image: nginx
      env:
       - name: APP_COLOR
         valueFrom:
           configMapKeyRef:
             name: myapp-config
             key: APP_COLOR
```

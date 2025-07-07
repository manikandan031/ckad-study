## Readiness Probe
* Pod Status - `Pending`, `ContainerCreating`, `Running`
* Pod Conditions - 
  * `PodScheduled` - Pod scheduled to a node 
  * `Initialized` - All init containers have completed successfully
  * `ContainersReady`- All containers in the pod are ready
  * `Ready` - Pod able to serve requests
* Readiness Probes - Http, TCP, Exec
```yaml
spec:
  containers:
    - image: nginx
      name: nginx
      ports:
        - containerPort: 8080
      readinessProbe:
        initialDelaySeconds: 10
        periodSeconds: 5
        failureThreshold: 8 # Default 3
        httpGet:
          path: /api/ready
          port: 8080
        tcpSocket:
          port: 3306
        exec:
          command:
            - cat
            - /api/isReady
```
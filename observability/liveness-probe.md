## Liveness Probe
* To periodically check if the container is healthy. By default, as long as the container is running kubernetes assumes it is healthy. This may not always work for example if the application is stuck is an infinite loop.
* Configuration is similar to readinessProbe.
```yaml
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 8080
      livenessProbe:
        initialDelaySeconds: 10
        periodSeconds: 5
        failureThreshold: 8
        httpGet:
          path: /api/health
          port: 8080
        tcpSocket:
          port: 3306
        exec:
          command:
            - cat
            - /api/is_healthy
```
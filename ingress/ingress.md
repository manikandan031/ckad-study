## Ingress
* Purpose
  * route external traffic to services based on `host` or `path`.
  * SSL configuration
* There are 2 things
  * `Ingress Controller` - Tool to handle the traffic routing. This is not installed by default with kubernetes. commonly used tools include nginx, HAProxy, Istio etc.
  * `Ingress resources` - K8s definition files that configure the ingress rules.
```yaml
apiVersion: networking.k8s.io/v1 
kind: Ingress
metadata:
  name: my-app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /  # this is needed to rewrite the http path sent to the target service
    nginx.ingress.kubernetes.io/ssl-redirect: "false" # Avoid redirect to ssl port 
spec:
  rules:
    - http:
        paths:
          - path: /wear
            pathType: Prefix
            backend:
              service:
                name: wear-service
                port:
                  number: 8282
```

```shell
# Ingress rules from cmd
k create ingress my-app-ingress -n app-space --rule="/wear=wear-service:8080" --rule="/watch=video-service:8080"
```
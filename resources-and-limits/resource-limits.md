### Resources and Limits
* Every container in a pod can have a `Request` and `Limit` for cpu and memory. 
* If none specified in the configuration then there is no request and no limit. Such containers can consume all the resources in the node and starve other containers.
* Requests - if specified guarantees the resource for the container when it is created
* Limits - if specified is the max. resource that gets allocated to the container. beyond which cpu is throttled or out of memory kill happens.
* Ideal config - set a request without a limit. In this case, additional resources will be used if available.
```yaml
spec:
  containers:
    - image: nginx
      name: nginx
      resources:
        requests:
          cpu: 1
          memory: '2Gi'
        limits:
          cpu: 2
          memory: '8Gi'
```
* Namespace level configuration to limit for all pods - `LimitRange`
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
      # default limit if no configuration for a container
      - default:
          cpu: 1
        # default request if no configuration for a container
        defaultRequest:
          cpu: '500m'
        # if configured in a container, k8 will check if it is above the minimum values
        min:
          cpu: '100m'
        # if configured in a container, k8 will check if it is below the maximum values 
        max:
          cpu: 2
        type: Container
```
* Global limit for the entire cluster - `ResourceQuotas`
```yaml
apiVersion: v1
kind: ResourceQuota 
metadata:
  name: my-resource-quota
spec:
  hard:
    requests.cpu: 4
    limits.cpu: 8
    requests.memory: '10Gi'
    limits.memory: '20Gi'
```
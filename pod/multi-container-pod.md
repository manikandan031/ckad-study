## Multi Container Pods
* Patterns - sidecar, ambassador, adaptor
* share same network space and volumes. can reach each other `localhost`
* `initContainers` - these are run before starting the actual containers. they must do init tasks and exit to completion.
```yaml
spec:
  initContainers:
    - image: busybox
      name: bb
      command: ["git", "clone", "<repo"]
  containers:
    - image: nginx
      name: nginx
    - image: log-agent
      name: log-agent
```
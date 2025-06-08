### SecurityContext
* By default the process inside docker containers are run as `root` user. 
```shell
# run below command to see the user with which pid 1 is running inside the container
kubectl exec ubuntu-pod -- whoami
# or list the processes in the container or the host
ps aux 
```
* `Linux Capabilities` are used to perform specific actions. We can either add or drop capabilities to a docker container.
* `securityContext` configuration allows to override `user` and `capabilities`
```yaml
spec:
  securityContext:
    runAsUser: 1010
    containers:
      - image: ubuntu
        name: ubuntu
        command:
          - sleep
          - 5000
        securityContext:
          runAsUser: 1000
          capabilities:
            add: ["SYS_TIME"]
```
## Pod Configuration - Commands and Arguments

Dockerfile
```
FROM ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
```

Pod definition
```yaml
apiVersion: v1
kind: Pod
metadata:
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      args: ["5"]
      command: ["sleep2.0"]
```
Note: 
In the above definition `args` override the `CMD` in Dockerfile and 
`command` overrides the `Entrypoint` in Dockerfile.
The first item in the command array should be the executable command

### Commands - override Entrypoint and CMD
```bash
ENTRYPOINT ["python", "app.py"]
CMD ["--color", "blue"]

# run pod with color green
kubectl run webapp-green --image=webapp -- --color green

#override entrypoint for example run `python app2.py`
kubectl run webapp-green --image=webapp --command -- python app2.py --color green

```
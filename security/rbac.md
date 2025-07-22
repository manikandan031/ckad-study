## RBAC
ROLE
* Roles define permissions within a namespace
* role bindings map roles to subjects (users/groups)

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
  - apiGroups: [""] # core apis 
    resources: ["pods"]
    verbs: ["list", "get", "create", "update", "delete"]
  - apiGroups: ["apps"] # named apis 
    resources: ["deployments"]
    verbs: ["create"]
```
ROLE-BINDING
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata: 
  name: deev-user-role-binding
subjects:
  - kind: User
    name: dev-user
    apiGroup: rbac.authorization.k8s.io/v1
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io/v1
```
```shell
kubectl get roles
kubectl get rolebindings
kubectl describe role developer
k create role developer --verb=list --verb=create --resource=pods
k create rolebinding dev-role-binding --role=developer --user=dev-user
```
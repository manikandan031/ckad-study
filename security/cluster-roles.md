## Cluster Roles
* resources are either `Namespaced` or `Cluster Scoped`. 
* Pods, ReplicaSets, Deployments, Secrets, ConfigMaps, PVC, Role, RoleBindings are namespaced.
* Nodes, ClusterRole, ClusterRoleBinding, Namespaces, CertificateSigningRequest are Cluster Scoped.
* a cluster role can be applied to a namespaced resource. in that case, the user can access the resource across namespaces.
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: node-admin
rules:
  - apiGroups: [""]
    resources: ["nodes"]
    verbs: ["get", "create", "delete"]  
```
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: node-admin-binding
subjects:
  - kind: User
    name: node-admin-user
    apiGroup: rbac.authorization.k8s.io
roleRef:
  name: node-admin
  kind: ClusterRole
  apiGroup: rbac.authorization.k8s.io
```

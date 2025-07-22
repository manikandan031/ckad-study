## API Groups
* kube api server can be accessed @ `https://<master-node-ip>:6443/`
* APIs - `/metrics`, `/healthz`, `/api`, `/apis`, `version`, `logs`
* apiGroups
  * `/api` - core apis
    * /v1
    * resources
      * /pods
        * verbs - "list", "get", "create", "update", "delete"
      * /namespaces
      * /rc
      * /pv
      * /pvc
      * /configmaps
      * /secrets
      * /nodes
      * /events
      * /services
  * `/apis` - named
    * /apps
      * /v1
        * /deployments
        * /replicasets
        * /statefulsets
    * /extensions
    * /networking.k8s.io
      * /v1
        * /networkpolicies
    * /certificates.k8s.io
      * /v1
        * /certificatesigningrequests
* to use curl pass `--key` , `--cert` and `--cacert`
* run `kubectl proxy` command to create a local proxy server. this uses the certs from the kubeconfig file. then curl `http://localhost:8001`
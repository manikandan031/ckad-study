## Kubeconfig
* default path $HOME/.kube/config
* contains 3 sections
  * clusters
  * users
  * contexts
* currentContext - specify the default context to be used with kubectl commands
* certificates
  * certificateAuthority - specified for the cluster
  * clientKey - specified for the user
  * clientCertificate - specified for the user
```shell
#use this command to check api server options. for example authorization modes
k describe pod kube-api-server -n kube-system
```
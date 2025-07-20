## authentication
* kube-api-server authentication Options
  * basic-auth-file 
    * csv file with passwords, username, group in plain text. 
    * `-- basic-auth-file`option passed to kube-api-server pod
    * `curl -v -k https://master-node-ip:6443/api/v1/pods -u "user:password"`
  * token-auth-file
    * csv file with tokens, username, group in plain text. 
    * `-- token-auth-file` option passed to kube-api-server pod.
    * `cutl -v -k https://master-node-ip:6443/api/v1/pods --header "Authorization: Bearer <token>"`
  * certificates
  * identity providers
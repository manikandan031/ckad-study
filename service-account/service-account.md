### Service Account
* User Account - for human users, Service account - for machine users. 
* Example, Jenkins uses machine user to deploy application in kubernetes.
```shell
# create service account
kubectl create serviceaccount my-service-account
kubectl get serviceaccount
kubectl describe serviceaccount my-service-account
```
* kubernetes creates a `default` service account for each namespace in the cluster
* Prior to v1.22, 
  * For each service account a service-account-token is automatically created and stored as a secret. 
  * The auto-generated token does not have any expiry and no audience.
  * secret type `kubernets.io/service-account-token`
  * This token in the secret can be used as a bearer token to authenticate with the api server.
  * For every pod, by default this secret is volume mounted into the pod in a predefined path `/var/run/secrets/kubernetes.io/serviceaccount`
  * To mount a custom service account, specify `serviceAccountName` in pod-definition spec.
```yaml
spec:
  containers:
    - image: nginx
      name: nginx
  serviceAccountName: my-service-account
  # below config to disable automatic volume mounting of service account token
  automountServiceAccountToken: false 
```
* After v1.22, 
  * creating a service account does not create a indefinite living token as a secret. 
  * Instead, Token request api will be used to create tokens which will have an expiry. They are mounted as projected volume with an expiry in the same path.
* After v1.24
  * Token is not created by default. We must explicity run `kubectl create token my-service-account`. This will create a token with an expiry
  * To still create a secret with a long living token. create a secret like below.
```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
  name: my-sa-token
  annotations:
    kubernetes.io/service-account.name: my-service-account
```

### Secrets
* create secret 
```sh
#note: use plain text values in command. they are automatically base64 encoded
kubectl create secret generic my-secret --from-literal=db_host=mysql.host.com 
```
* view secret
```sh
# get secret with data in base64 encoded format. 
kubectl get secret my-secret -o yaml
# decode secret
echo '<secret-val>' | base64 --decode
```
* inject all secret into pod
```yaml
spec:
  containers:
    - name: nginx
      image: nginx
      envFrom:
        - secretRef:
            name: my-secret
```
* inject specific secret into pod
```yaml
spec:
  containers:
    - name: nginx
      image: nginx
      env:
        - name: db_host
          secretKeyRef: 
             name: my-secret
             key: db_host
```
* inject secrets as files (volumes). Each secret key is created as a file with the value.
```yaml
volumes:
  - name: app-volume
    secret:
      secretName: app-secret
```
* By default secrets are not encrypted at rest in etcd. Running below command in the control plane will display the secret unencrypted
```shell
apt-get install etcd-client
ETCDCTL_API=3 etcdctl \
   --cacert=/etc/kubernetes/pki/etcd/ca.crt   \
   --cert=/etc/kubernetes/pki/etcd/server.crt \
   --key=/etc/kubernetes/pki/etcd/server.key  \
   get /registry/secrets/default/secret1 | hexdump -C
```
* To check whether encryption is already enabled, inspect the `--encryption-provider-config` of the kube api server. In the control plane run below command
```shell
ps aux | grep "kube-apiserver" | grep "encryption-provider-config"
```
* the kube api server manifests file will also have these options. To configure a encryption configuration, 
1. create the EncryptionConfiguration yaml
2. provide the path in the kube-apiserver manifests file.
3. Configure volume mounts in the manifests file.
```shell
# check if this has kube-apiserver.yml
ls /etc/kubernetes/manifests
```
```yml
#Encryption configuration
---
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - aescbc:
          keys:
            - name: key1
              secret: <BASE 64 ENCODED SECRET>
      - identity: {} # this fallback allows reading unencrypted secrets;
```
```sh
# Generate encryption key 32bit random and update the above yaml.
head -c 32 /dev/urandom | base64
```
```yml
# Above configuration yaml should be volume mounted into the api-server pod. 
# Edit the /etc/kubernetes/manifests/kube-apiserver.yml to add `--encryption-provider-config` option and the volume mount.
---
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: 10.20.30.40:443
  creationTimestamp: null
  labels:
    app.kubernetes.io/component: kube-apiserver
    tier: control-plane
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
    - command:
        - kube-apiserver
      ...
      - --encryption-provider-config=/etc/kubernetes/enc/enc.yaml  # add this line
      volumeMounts:
      ...
      - name: enc                           # add this line
        mountPath: /etc/kubernetes/enc      # add this line
        readOnly: true                      # add this line
      ...
  volumes:
  ...
  - name: enc                             # add this line
    hostPath:                             # add this line
      path: /etc/kubernetes/enc           # add this line
      type: DirectoryOrCreate             # add this line
  ...
```
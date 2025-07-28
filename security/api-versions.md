## API Versions
* api versions 
  * v1 - GA / stable
  * v1Beta1 - enabled by default. can have minor bugs.
  * v1Alpha1 - disabled by default. can have bugs.
```shell
kubectl explain deployment
```
* apiGroups can support multiple apiVersions at the same time. for example, apps can support `/v1`, `/v1Beta1`, `/v1Alpha1`. So user can create a deployment with any of the version in the yaml.
* preferredVersion - `curl http://127.0.0.1/apis/batch`. look for `preferredVersion` in response
* when we run `k get deployment` which version should be queried? preferredVersion defines it.
* storageVersion - version stored in etcd.
* to enable an alpha version api. add it to `--runtime-config` parameter of api-server.

## API Deprecation rules
* Alpha versions can be immediately removed
* Beta versions should be supported for atleast 3 release / 9 months. meanwhile they can be deprecated.
* Deprecated version still remains as the preferred / storage version for one more release.
* stable version can only be deprecated if another stable version is available.
* use `kubectl convert` command to convert from one version to another. its a plugin and should be installed before using it.
```
kubectl api-resources (to get short names)

kubectl explain job (returns api details)

kubectl proxy 8001& # & runs the command in the background
curl localhost:8001/apis/authorization.k8s.io

kubectl-convert -f ingress-old.yaml --output-version networking.k8s.io/v1 
```
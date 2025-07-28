## Admission Controller
* Admission controllers run after authentication and authorization to validate configurations, alter requests before running actual command.
* By default few admission plugins are enabled and others are disabled.
* Configured in kube-api-server
```shell
# Check admission plugins
kubectl exec kube-apiserver-controlplane -- kube-apiserver -h | grep enabled-admission-plugins
# inspect enabled-admission-plugins, disabled-admission-plugins options
cat /etc/kubernetes/manifests/kube-apiserver.yaml
```
* Types
  * Mutating Admission Controller - changes the request
  * Validating Admission Controller - validates the request and allow/deny
  * Mutating controllers are run first before the validating controllers so that any mutations are also validated
  * To add custom controllers - MutatingAdmissionWebHook, ValidatingAdmissionWebHook
    * Deploy Webhook server
      * 2 APIs - /mutate, /validate
      * response field `allowed` - true to allow and false to deny
    * Create ValidatingWebhookConfiguration object and point it to the webhook server
    * they will be invoked after all the built in admission controllers are run
```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: "pod-policy.example.com"
webhooks:
  - name: "pod-policy.example.com"
    clientConfig:
      #url: 'https://external.api.com'
      service:
        name: "webhook-service"
        namespace: "webhook-namespace"
      caBundle: ""
    rules:
      - apiGroups: [""]
        apiVersions: ["v1"]
        operations: ["CREATE"]
        resources: ["pods"]
        scope: "Namespaced"
      

```
  
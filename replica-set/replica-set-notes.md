## Replica Set
- *apiVersion* should be `apps/v1`
- *spec* 
  - *selector* with *matchLabels* to select pods to be included in the replicaSet.
  - *template* - the pod definition
  - *replicas* - number of replicas
- kubectl scale command example
    ```
     kubectl scale rs my-replica-set --replicas=4
    ```
- kubectl get object as yml file
    ```
     kubectl get rs my-replica-set -o yaml > my-replica-set.yml
    ```
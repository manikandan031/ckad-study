## Stateful Sets
* usecases -  to start/stop pods one after the other sequentially, to assign unique servicenames for each pod.
* Example - mysql cluster - 1 master (read/write), 2 slaves (read replicas)
* definition is exactly same as deployment.
* additional options
  * podManagementPolicy - parallel (to start pods in parallel)
  * volumeClaimTemplates - template for a PVC. to create separate claims for each pod.
## Deployment
* kubernetes object for managing deployments
  * rolling updates
  * rollback
  * pause / resume
* definition is same as `ReplicaSet` except `kind`
* Rollout - process of deploying a new version of the application
* Revision - Each rollout creates a revision
````shell
# create deployment
kubectl create deployment nginx --image=nginx:1.6.1
# view rollout status
kubectl rollout status <deployment>
# view revisions
kubectl rollout history <deployment>
# revisions contain a change-cause. by default it is not recorded. use the --record flag to record the change-cause.
kubectl set image deployment/<my-deployment> nginx=nginx:1.7 --record 
````
* Deployment Strategies
  * Recreate - destroy all pods in current version and then create new version pods. results in downtime. Not the default.
  * RollingUpdate - destroy current version pod 1 at a time and create new versions. Default.
* Rollback
  * Under the hood, kubernetes create a second replica set, scales down the current rs and scales up the new rs during a deployment.
  * When rolling back, the second rs will be scaled down and the pods will be recreated in the first rss
````shell
# Rollback to previous revision
kubectl rollout undo <deployment>
# Rollback to specific revision
kubectl rollout undo <deployment> --to-revision=2
````
* Blue/Green and Canary Deployment
  * These are not directly configurable settings in kubernetes. they are achieved with services, labels and selectors.
  * Blue-Green
    * 2 Deployments are deployed for the application - app-v1 and app-v2
    * In each deployment the pods have labels
      * app-v1 -> app=frontend, version=blue
      * app-v2 -> app=frontend, version=green
    * The Service selector initially selects pods with `version=blue`. So all traffic is routed to blue. After deployment and successful validation, traffic is switched to new version by updating the service to pick pods with label `version=green`
  * Canary
    * Small percentage of traffic is routed to new version. if there are no errors, gradually all traffic is routed to the new version. Finally,old deployment is deleted.
    * K8 does not offer direct control over traffic percentage. This is controlled by the number of pods. simply use small replica set for the new version.

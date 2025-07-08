## Jobs and CronJobs
* `docker run ubuntu expr 3+2` - this container will evaluate the expression and immediately exits (with code 0 indicated successful completion). But kubernetes will attempt to restart the pod to try and keep it running.
* For such tasks that need to go to completion use `Job`. Jobs run immediately, if you want to run them on a Schedule use `CronJob`
* pod configuration - `restartPolicy`
  * Always - Default
  * Never 
  * OnFailure - If exit code is not 0
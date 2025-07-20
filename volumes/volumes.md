## Volume, PersistentVolume and PersistentVolumeClaim
Volumes and Volume Mounts. Volume Definition is inline along with Pod spec.
```yaml
spec:
  containers:
    - image: nginx
      name: nginx
      volumeMounts:
        - name: config-vol
          mountPath: /opt
          readOnly: true
  volumes:
    - name: config-vol
      hostPath: /data
      type: Directory
```
Persistent Volumes are used to centrally create a pool of volumes at cluster level. Pods then claim pieces of volume from the pool.
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: config-vol
spec:
  capacity:
    storage: 5Gi
  accessModes: 
    - ReadWriteOnce  #( Volume can be accessed as Read/Write by a single Node)
  persistentVolumeReclaimPolicy: Recycle
```
Persistent Volume Claims are used to claim a PersistentVolume based on criteria
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: config-vol-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
  selector:
    matchLabels:
      name: config-vol
```
Define PersistentVolumeClaim as Volume in pod definition and mount into the pod
```yaml
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: my-config-vol
          mountPath: /opt
  volumes:
    - name: my-config-vol
      persistentVolumeClaim:
        claimName: config-vol-claim
        
```
* `emptyDir` is used to create a volume and mount to a pod a scratch space. when the pod is moved out of the node, the data is lost.
* `storageclass` is used to dynamically create PVs when a PVC is created using provisioners from different cloud providers

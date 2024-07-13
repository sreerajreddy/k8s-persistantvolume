-First we have create a pod with mongo image which is on port 27017. ans is port forwarded to 32000.
- We're using  mongodb compass(which is a mongo client GUI) to connect to the database.
-Any chages made in the container doesn't apply to the container when the pods restarts for some reason, to overcome that issue we use volume at pod leve called emptyDir. So, wehenever a pod is restarted a new contaienr is created which results in shared volume of the previous container, whichever changes has been made to the container reflects and getes updated in the new container.
(this is not the ultimate solution for this)


>>kubectl get pod <pod-name> -o yaml (to get the details and id of the pod)
>>kubectl get pod mongo-6b55bc7b-bz59m -o jsonpath='{.metadata.uid}' (to filter the pod id)

-Whenever the pods is deleted the entire data associated to that pod gets deleted.
-Solution for this is hostPath. where it mounts a file/dir from the host(node) filesystem to the node. So, all the pods createing in the node has the shared volume. Even if the pods gets deleted data will be stored in the node.

Spec:

 ...

 Volumes:
  - name: volume
    hostPath: 
      path: /data/db '''the path where the data should be stored in the node.'''

-The alternate way is to host the data in the cloud or NFS like AWS EBS.
-This can be acheived in three ways,
 PersistentVolumes
 PersistentVolume Claim
 Storage class

-

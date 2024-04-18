# LAB 4.1 emptyDir volume. Copying files. Run a shell in an existing pod.

Review the manifest *simple-pod-emptyDirVolume.yaml*

!!! Note the multiple mountpoints. 

 And apply it:
 ```
 kubectl apply -f simple-pod-emptyDirVolume.yaml 
 ```

***TASK:*** Review kubectl cp syntax and usage.
 Copy a local file to the running pod, at the /cache location.

 
 Run the below command to execute a shell into a running pod:
 ```
 kubectl exec -it alpine -- /bin/sh
 ```

 SYNTAX: ``kubectl exec -it <pod_name> (-c <container_name>) -- <command>``
/ similar to docker exec /

List the /cache and /second_cache mount points.


# Lab 4.2 PV and PVC

Examine the *lab42_pod.yaml* manifest:

Apply it:
```
kubectl apply -f lab42_pod.yaml 
```

Get the pod status and the reason.

Create the PVC:
```
kubectl apply -f lab42_PVC.yaml 
```

List PVCs:
```
kubectl get pvc
```

List PVs:
```
kubectl get pv
```

**Question:** Where is the admin? How the volume was privisioned???


# LAB 43 Playing with StorageClasses

List the current storage classes:

```
kubectl get storageclasses
```

Enable csi hostpath driver:
```
minikube addons enable csi-hostpath-driver
```

Again list the storage classes:
```
kubectl get storageclasses
```

Review *lab43_my_custom_storageClass.yaml*

```
less lab43_my_custom_storageClass.yaml 
```

Apply it:
```
kubectl apply -f lab43_my_custom_storageClass.yaml 
```

Again list the current storage classes:

```
kubectl get sc
```

Review *lab43_PVC.yaml *

Now create a PVC using the new storage class:
```
kubectl apply -f lab43_PVC.yaml 
```

Get the PVC status:
```
kubectl get pvc
```
**NOTE:** It should be pending because there is no pod using it.

Check the PVC status:
```
kubectl describe pvc my-custom-pvc
```

Create a the pod that uses this class:
```
kubectl apply -f lab43_pod.yaml 
```


List PVCs, PVs and PODS


Delete the POD with the custom storageclass:
```
kubectl delete -f lab43_pod.yaml 
```

Delete the PVC with the custom storage class:
```
kubectl delete -f lab43_PVC.yaml 
```

Note the volume status. Should be RELEASED.


# LAB 4.4 StatefulSet


Review the manifest *lab44_statefulSet.yaml*

Apply it:
```
kubectl apply -f lab44_statefulSet.yaml
```

# LAB 4.5 ConfigMap as volume

Create the ConfigMap:
```
kubectl apply -f lab45_sonfigMap.yaml
```

Review the pod definition. Pay attention to env. var and volume definitions.

Create the POD:
```
kubectl apply -f lab45_POD_volume_envs_ConfigMap.yaml
```

Go insite the pod to look around:
```
kubectl exec -it lab45-pod -- /bin/sh
```

List the /config location
List the env. variables.


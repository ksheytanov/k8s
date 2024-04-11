# LAB 2.1 CMD overrides and env variables

Examine the [docker image for nginx](https://github.com/nginxinc/docker-nginx/blob/1.14.2/stable/stretch/Dockerfile), take a look at the CMD directive.



Examine the pod-argsonly.yaml manifest:
```
cat pod-argsonly.yaml
```

Apply the manifest:
```
kubectl apply -f pod-argsonly.yaml 
```

Examine the newly created pod. 

**Construct the command to examine a pod by yourself. :)**

Run the following command to start the same pod wuth kubectl run:

```
kubectl run nginx-argsonly-shell --image=nginx:1.14.2 --port=80 -- nginx -g "daemon off;"
```
NOTE the commands after the two dashes "--"


Again examine the new pod:

**Construct the command to examine a pod by yourself. :)**

Create a new pod, but this time from the *pod-cmdargs-override.yaml* manifest

Examine the pod and notice the difference in the startup command.

***

Review the file *args-and-envs.yaml*

 \
Note the **arguments**.\
Note the **environment variable**.\
Note the **image** being used.

 \
Create the resources from the manifest.
```
kubectl apply -f args-and-envs.yaml
```

Get the logs from the newly created pod:
```
kubectl logs simple-messenger
```

Tail the logs by adding -f at the end.
```
kubectl logs simple-messenger -f
```

# LAB 2.2 Pod resource definition and Troubleshooting

 \

**!!!IMPORTANT:** Do not inspect the below manifest.\
\

Apply the pod-with-req.yaml manifest without inspecting it:

```
kubectl apply -f pod-with-req.yaml
```

**TASK1:** The above will create a pod named *"nginx-requests"*.
 - What is the status of the pod?
 - Can you try to figure out why?
 
 


# LAB 2.3 playing with replicaSets 

Open the manifest *replicaSet.yaml*  and review it.

How many pods will be created?

Apply the  *replicaSet.yaml* manifest:

```
kubectl apply -f replicaSet.yaml 
```

List pods

List Replicasets:
```
kubectl get rs
```

Delete one of the newly created pods.

Get the cluster events:
```
kubectl get events --sort-by=.metadata.creationTimestamp
```

NOTE the sorting.

Create a simple pod but with the same label as the app in the replicaset:
```
kubectl apply -f simple-pod-with-label.yaml
```

List all pods

Descrine the replicaset:

```
kubectl describe rs/nginx-replicaset
```

Have a look at the events section

Scale the replicaset. Decrease the number of replicas by running:
```
kubectl scale --replicas=2 rs/nginx-replicaset
```

# LAB 2.4 Creating a deployment


Create a deployment by running:
```
kubectl create deployment mysite --image=nginx --replicas=3
```

Scale the deplopyment to 10 replicas:
```
kubectl scale deployment mysite --replicas=10
```

Update the deployment with a new image:
```
kubectl set image deployment mysite nginx=myfakeimage333333
```

List pods.
Describe deployment.

Revert to previous version of deployment:
```
kubectl rollout undo deployment mysite
```
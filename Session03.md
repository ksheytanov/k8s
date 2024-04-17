# LAB 3.1 Playiong with rollouts

Run the following commands to create a deployment and trigger two rollouts
```
kubectl apply -f DeplRollout.yaml
kubectl set image deployment/mysite2 nginx=nginx:1.25.4-alpine-slim --record=true
sleep 5
kubectl set image deployment/mysite2 nginx=nginx:BUUUU --record=true
```

Monitor the rollout:
```
kubectl rollout status deployment/mysite2
```

Review rollout history:
```
kubectl rollout history deployment/mysite2
```

Rollback to a specific revision:
```
kubectl rollout undo deployment/mysite2 --to-revision=2
```

# LAB 3.2 ClusterIP Service and exposure

Create a simple deployment and a service:

Open the manifest to review it. Note the port definitions.
```
kubectl apply -f DeplWithService.yaml 
```


Run an interactive shell from alpine image:
```
kubectl run -i -t mypod --image=alpine:3.12 --restart=Never
```
NOTE: take a note of the option (-i -t --restart=Never)
 \

Install curl
```
apk --update add curl
```

Execute
```
curl nginx-service
```

Type ``exit`` to end the session.

List pods
```
kubectl get pods
```
Note the status of ``mypod``

Copy the name of one of the nginx pods and run:
```
kubectl port-forward pod/<POD_NAME> 3333:80
```
NOTE: substitute POD_NAME

Expose a deployment and a service using the same approach:
```
kubectl port-forward deployment/mysite2 3333:80
```
or
```
kubectl port-forward deployment/mysite2 3333:80
```
# LAB 3.3 Headless service

Review the file *HeadlessService.yaml*

Create the reasource in *HeadlessService.yaml*
```
kubectl apply -f HeadlessService.yaml
```

Run again the interactive shell from alpine:
```
kubectl run -i -t mypod --image=alpine:3.12 --restart=Never
```

Run:
```
nslookup headless-nginx
```

Scale the deployment to 5 replicas:
```
kubectl scale deployment mysite2 --replicas=5
```

Again run nslookup:
```
nslookup headless-nginx
```

# LAB 3.4 LoadBalancer service

TASK: Edit DeplWithService.yaml file to make the service inside it of type LoadBalancer

Delete all resources created today:

```
kubectl delete -f .
```
NOTE: The synax used -> -f option can take a path as value

Apply the modified manifest to create a loadbalancer servive:

```
kubectl apply -f DeplWithService.yaml
```

List current services
```
 kubectl get svc
 ```
 Note the IPs of the service


 In another shell run:
 ```
 minikube tunnel
 ```

 Go back and list services again. Check external IP

# Lab 1.1 - Installing minikube

Navigate to the [Getting Started page](https://minikube.sigs.k8s.io/docs/start/) on official minikube site.

Install with homebrew:
```
brew install minikube
```

Check after install:
```
which minikube
```

**IMPORTANT**: Do not hurry to start minikube. We would need to switch to the docker runtime driver. To do so run:

```
minikube start --driver=docker
```

To make docker the default driver run

```
minikube config set driver docker
```

> Taken from the [official docker driver docs.](https://minikube.sigs.k8s.io/docs/drivers/docker/)

List minikube addons:
```
minikube addons list
```

Enable the kubernetes dashboard addon:
```
minikube addons enable dashboard
```

Enable metrics server addon:
```
minikube addons enable metrics-server
```

Open local minikube dashboard by running:
```
minikube dashboard
```
Look around in the Dashboard. Navigate to the kube-system namespace to view the system pods running kubernetes services.

# Lab 1.2 - First Cluster Interactions

Cd into Session01 folder. View the pod definition manigest with a missing field:
```
cat simple-pod_missing_data.yaml
```
**Notice the commented lines (Starting with a #) symbol.**
 

***


Perform a dry run with the invalid manifest:
```
kubectl apply -f simple-pod_missing_data.yaml --dry-run=server
```
Observe the response from the server.

***

Perform a dry-run with the correct manifest:
```
kubectl apply -f simple-pod.yaml --dry-run=client
```

Now lets really apply the manifest:
```
kubectl apply -f simple-pod.yaml 
```
***
List the currently running pods:
```
kubectl get pods
```


Get detailed information about a particular pod (nginx):
```
 kubectl describe pod nginx
```
**NOTE:** Have a look at the general section, the Containers and the Events sections. Pay attention which event comes from which service.

***
Delete a pod by directly specifying pod to be deleted:
```
kubectl delete pod nginx
```

Run a the same pod, but not from the manifest but with the run command:
```
kubectl run nginx --image=nginx:1.14.2 --port=80
```
NOTE: Take a look at the dashboard

Edit the pod definition live (change image tag to latest):
```
kubectl edit pod nginx
```
Observe the POD events

Delete the POD by passing a manifest file to the delete command:
```
kubectl delete -f simple-pod.yaml
```
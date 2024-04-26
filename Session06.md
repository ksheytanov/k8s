# LAB 6.1 Playing with contexts
List current context:
```
kubectl config current-context
```

List available contexts:
```
kubectl config get-contexts
```

Review kubectl's configuration file ``~/.kube/config``
```
cat ~/.kube/config
```

## [O] Install GCLOUD CLI and provision cluster:

You can install GCLOUD CLI by Following [the official guide.](https://cloud.google.com/sdk/docs/install-sdk)


[OPTIONAL]  Authenticate GCloud CLI:
```
gcloud auth login
```

[OPTIONAL - TAKES SOME TIME]  
Provision an AutoPilot Cluster in the default project and region:
```
gcloud container clusters create-auto my-gke-cluster
```

## Configure additional context in kubectl:

Check if GKE Auth plugin is installed:
```
gke-gcloud-auth-plugin --version
```

Install GKE Auth plugin:
```
gcloud components install gke-gcloud-auth-plugin
```

List available GKE clusters
```
gcloud container clusters list
```

Get credentials for a specific cluster and add them as kubectl context
```
gcloud container clusters get-credentials CLUSTER_NAME \
    --region=COMPUTE_REGION
```
**NOTE:** Substitute *CLSUTER_NAME* and *COMPUTE_REGION *

List available contexts again:
```
kubectl config get-contexts
```

Set the current context to point to GKE:
```
kubectl config set-context <CONTEXT_NAME>
```
List namespaces. List storageclasses.

You can switch to minikube like this:
```
kubectl config use-context minikube
```

# LAB 6.2 Gateway API


List currently provisioned Gateway Classes:
```
kubectl get gatewayclasses
```

Create a Gateway:
```
kubectl apply -f lab62Gateway.yaml
```
Monitor the provisioning status:
```
kubectl get gateways
```


Or with
```
kubectl describe gateways
```

Create a simple nginx deployment with a ClusterIP Service:
```
kubectl apply -f lab62DeplWithService.yaml
```

Review the route definition ``lab62HTTProute.yaml`` and apply it:
```
kubectl apply -f lab62HTTProute.yaml
```

Get the Gateway Address by running:
```
kubectl get gateways
```
Navigate to http://``<gateway-ip-address>``/vanilla-nginx

Create a second app - a customized nginx with a deplloyment and a service. Review the manifest ``lab62SecondApp.yaml``
```
kubectl apply -f lab62SecondApp.yaml
```

Review the manifest *lab62SecondHTTProute.yaml*

Create the route for the second app:
```
kubectl apply -f lab62SecondHTTProute.yaml 
```

Navigate to http://``<gateway-ip-address>``/other-app
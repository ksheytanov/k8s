# LAB 5.1 ReadOnlyMany - mount a single volume on many pods:

Review ***lab51_Depl.yaml***  file

And apply it:
```
kubectl apply -f lab51_Depl.yaml 
```

Check the status of the pods:
```
kubectl get pods
```

Review the ***lab51_PVC.yaml*** PVC and create it:
```
kubectl apply -f lab51_PVC.yaml 
```

Run
```
kubectl get pods
```

Take note of one of the pod names and expose it:
```
kubectl port-forward <PODNAME-GOES_HERE> 3434:80
```
NOTE: Substitute the pod name in the above command.

visit http://localhost:3434

Copy the custom html to one of the pods:
```
kubectl cp lab51_index.html \
<PODNAME-GOES_HERE>:/usr/share/nginx/html/index.html
```
Refresh the page http://localhost:3434

SCALE THE DEPLOYMENT TO 0

Refresh the page

SCALE IT BACK TO 2

Expose the deployment:
```
kubectl port-forward deployment/lab51-deployment 3434:80
```

# LAB 5.2 Consuming a ConfigMap

Create the ConfigMap:
```
kubectl apply -f lab52_configMap.yaml
```

Get configMaps.

Describe the configmap to review its contents:
```
kubectl describe configmap lab52-configmap-demo
```

Review the pod definition. Pay attention to env. var and volume definitions.

Create the POD:
```
kubectl apply -f lab52_POD_volume_envs_ConfigMap.yaml
```

Go insite the pod to look around:
```
kubectl exec -it lab52-pod -- /bin/sh
```

List the /config location \
List the env. variables.


# LAB 5.3 Secrets

Run the following command to create a generic secret:

```
kubectl create secret generic lab53-cred \
--from-file=lab53username=lab53user \
--from-file=lab53password=lab53pass \
--namespace=default
```
List secrets:
```
kubectl get secrets
```

Describe a secret:
```
kubectl describe secret lab53-cred
```

Get secret data:
```
kubectl get secret lab53-cred -o jsonpath='{.data}'
```

Generate a private/public key pair:
```
openssl genrsa -out lab52key.pem 2048 \
openssl rsa -in lab52key.pem -pubout -out lab52public.key
```

Create another secret from the key files:
```
kubectl create secret generic lab53-keys \
--from-file=lab52key.pem \
--from-file=lab52public.key 
```

**NOTE:** When key names are missing, the filenames are used for naming the keys!

Review the POD definition with secret references.

Apply the manifest:

```
kubectl apply -f lab53_POD_secrets.yaml
```

Run an interactive shell in the pod:
```
kubectl exec -it lab53-podsecrets -- /bin/sh
```

List env. variables.
List the mountpoint of where the keys are mounted.


# LAB 5.4 Helm
If helm is not installed you can install it with brew:
```
brew install helm
```

Go inside the lab54-helm folder
```
cd lab54-helm
```

Review the folder structure and the existing chart.

Chart was created with ``helm create <chartname>``

To check the generated manifests and do a test install run:
```
helm install --debug --dry-run testrelease ./mychartname
```

Install a release with custom values file:
```
helm install testrelease ./mychartname -f myDevValues.yaml 
```

Upgrade a release, use cusom values file, override a specific value:
```
helm upgrade testrelease ./mychartname \
-f myDevValues.yaml \
--set image.tag="1.24-alpine-slim"
```

List releases:
```
helm list
```

List release revisions:
```
helm history testrelease
```

Rollback to a specific version:
```
helm rollback testrelease 1
```






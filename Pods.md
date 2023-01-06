## Pods:
- It is an object in kubernetes.
- Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.

```md
# Commands Related to Pods

# How to Create Pods
1. kubectl apply -f simeplePod.yaml -n development 

# Get all pods in namespace
2. kubectl get pods -n development

# Get specific pod in namespace
3. kubectl get pods pod_name -n development

# Delete pods in namespace
4. kubectl delete pods nginx -n development

# Another way to delete pod is delete yaml file direclty.
5. kubectl delete -f simplePod.yaml

# Describe pods to check more details about pod
6. kubectl describe pods nginx -n development

# How to access a container in pod -- if pod contain single container
7. kubectl exec nginx -n development -it -- /bin/bash

# Return snapshot logs from pod nginx with only one container
8. kubectl logs nginx -n development

# Return snapshot logs from pod nginx with multi containers
9. kubectl logs nginx -n development --all-containers=true

# Return snapshot logs from all containers in pods defined by label app=nginx
10. kubectl logs -l app=nginx --all-containers=true -n development

# How to enter into pod with single container
11. kubectl exec nginx -it -- /bin/bash

# How to enter into pod with multiple containers
12. kubectl exec pod_name -c nginx -it -- /bin/bash
```
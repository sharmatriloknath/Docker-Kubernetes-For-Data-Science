# Deployments:
- A Deployment provides declarative updates for Pods and ReplicaSets.

- You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.


## Deployments Commands:

```md
# Create Deployment
kubectl apply -f .\Deployments\simpleDeployment.yaml

# Check what will be the output.
C:\Users\xyz\Desktop\ToDo\kubernetes>kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           11s

C:\Users\xyz\Desktop\ToDo\kubernetes>kubectl get rs
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-7fb96c846b   3         3         3       25s

C:\Users\xyz\Desktop\ToDo\kubernetes>kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-7fb96c846b-8gqkt   1/1     Running   0          34s
nginx-deployment-7fb96c846b-c225m   1/1     Running   0          34s
nginx-deployment-7fb96c846b-kcvhz   1/1     Running   0          34s

# Check Detail of Deployment
kubectl describe deploy nginx-deployment

# Scale Replaset of Deployment.
kubectl scale deploy nginx-deployment --replicas=5

# Autoscale the Deployment
kubectl autoscale deploy nginx-deployment --min=10 --max=15 --cpu-percent=80

# Update the Version of Image in Deployment
kubectl set image deploy nginx-deployment nginx=nginx:1.16.1

# Check Rollout status
kubectl rollout status deploy nginx-deployment

# Check rollout history
kubectl rollout history deploy nginx-deployment

# Get Previous Version 
kubectl rollout undo deployment/nginx-deployment

# Pause the Deployment
kubectl rollout pause deployment/nginx-deployment

# Resume the Deployment
kubectl rollout resume deployment/nginx-deployment
```
# Kubernetes
- It is an container management tool developed by google.
- It is an orchestration tool used to manage containers in cluster.


# 1. Namespaces
- It is a mechanism used to isolate the group of resoures in a single cluster.
- Namespace-based scoping is applicable only for namespaced objects (e.g. Deployments, Services, etc) and not for cluster-wide objects (e.g. StorageClass, Nodes, PersistentVolumes, etc).

#### Check the available namespaces
```cmd
kubectl get namespaces
kubectl get namespace
kubectl get ns
```

you will get some default namespaces.
```output
NAME              STATUS   AGE
default           Active   5d22h
kube-node-lease   Active   5d22h
kube-public       Active   5d22h
kube-system       Active   5d22h
```

**If you haven't created your namespace then you can start with the `default` one**

You can get resources from a particualar nampe space by providing `--namespace` or `--n` flag at the end.
```
kubectl get pods --namespace mynamespace
               or 
kubectl get pods -n mynamespace
```

### How to know Namespaced and Non-Namespaced Obejects.

**Namespaced:**
```
# In a namespace
kubectl api-resources --namespaced=true
NAME                        SHORTNAMES   APIVERSION                     NAMESPACED   KIND
bindings                                 v1                             true         Binding
configmaps                  cm           v1                             true         ConfigMap
endpoints                   ep           v1                             true         Endpoints
events                      ev           v1                             true         Event
limitranges                 limits       v1                             true         LimitRange
persistentvolumeclaims      pvc          v1                             true         PersistentVolumeClaim
pods                        po           v1                             true         Pod
podtemplates                             v1                             true         PodTemplate
replicationcontrollers      rc           v1                             true         ReplicationController
resourcequotas              quota        v1                             true         ResourceQuota
secrets                                  v1                             true         Secret
serviceaccounts             sa           v1                             true         ServiceAccount
services                    svc          v1                             true         Service
controllerrevisions                      apps/v1                        true         ControllerRevision
daemonsets                  ds           apps/v1                        true         DaemonSet
deployments                 deploy       apps/v1                        true         Deployment
replicasets                 rs           apps/v1                        true         ReplicaSet
statefulsets                sts          apps/v1                        true         StatefulSet
localsubjectaccessreviews                authorization.k8s.io/v1        true         LocalSubjectAccessReview
horizontalpodautoscalers    hpa          autoscaling/v2                 true         HorizontalPodAutoscaler
cronjobs                    cj           batch/v1                       true         CronJob
jobs                                     batch/v1                       true         Job
leases                                   coordination.k8s.io/v1         true         Lease
endpointslices                           discovery.k8s.io/v1            true         EndpointSlice
events                      ev           events.k8s.io/v1               true         Event
ingresses                   ing          networking.k8s.io/v1           true         Ingress
networkpolicies             netpol       networking.k8s.io/v1           true         NetworkPolicy
poddisruptionbudgets        pdb          policy/v1                      true         PodDisruptionBudget
rolebindings                             rbac.authorization.k8s.io/v1   true         RoleBinding
roles                                    rbac.authorization.k8s.io/v1   true         Role
csistoragecapacities                     storage.k8s.io/v1              true         CSIStorageCapacity



# Not in a namespace
kubectl api-resources --namespaced=false

NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
componentstatuses                 cs           v1                                     false        ComponentStatus
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v1                                     false        Node
persistentvolumes                 pv           v1                                     false        PersistentVolume
mutatingwebhookconfigurations                  admissionregistration.k8s.io/v1        false        MutatingWebhookConfiguration
validatingwebhookconfigurations                admissionregistration.k8s.io/v1        false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds     apiextensions.k8s.io/v1                false        CustomResourceDefinition
apiservices                                    apiregistration.k8s.io/v1              false        APIService
tokenreviews                                   authentication.k8s.io/v1               false        TokenReview
selfsubjectaccessreviews                       authorization.k8s.io/v1                false        SelfSubjectAccessReview
selfsubjectrulesreviews                        authorization.k8s.io/v1                false        SelfSubjectRulesReview
subjectaccessreviews                           authorization.k8s.io/v1                false        SubjectAccessReview
certificatesigningrequests        csr          certificates.k8s.io/v1                 false        CertificateSigningRequest
flowschemas                                    flowcontrol.apiserver.k8s.io/v1beta2   false        FlowSchema
prioritylevelconfigurations                    flowcontrol.apiserver.k8s.io/v1beta2   false        PriorityLevelConfiguration
ingressclasses                                 networking.k8s.io/v1                   false        IngressClass
runtimeclasses                                 node.k8s.io/v1                         false        RuntimeClass
clusterrolebindings                            rbac.authorization.k8s.io/v1           false        ClusterRoleBinding
clusterroles                                   rbac.authorization.k8s.io/v1           false        ClusterRole
priorityclasses                   pc           scheduling.k8s.io/v1                   false        PriorityClass
csidrivers                                     storage.k8s.io/v1                      false        CSIDriver
csinodes                                       storage.k8s.io/v1                      false        CSINode
storageclasses                    sc           storage.k8s.io/v1                      false        StorageClass
volumeattachments                              storage.k8s.io/v1                      false        VolumeAttachment
```

### Create A new namespace:
```yaml
# Create A Yaml File with filename.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: <insert-namespace-name-here>

# Run This Command
kubectl apply -f filename.yaml

                OR
kubectl create namespace <namespace-name>
```

### Delete Namespace:
```cmd
kubectl delete namespaces <insert-some-namespace-name>
```

### Get of Create Resources in a Namespace:
```cmd
kubectl get deployment -n <namespace-name>
kubectl get pods --namespace <namespace-name>

kubectl create deployment nginx-demo --image=niginx -n <namespace-name>
```

### How to configure Resources Quotas for Exach Namespace
- Used to manage the cluster resources in an efficent way.
- For futher details https://kubernetes.io/docs/concepts/policy/resource-quotas/

```yaml
# compute-resources.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi

# Run This command
kubectl apply -f compute-resource.yaml -n <namespace-name>

# count_objs.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: object-counts
spec:
  hard:
    configmaps: "10"
    persistentvolumeclaims: "4"
    pods: "4"
    replicationcontrollers: "20"
    secrets: "10"
    services: "10"
    services.loadbalancers: "2"


# Run This command
kubectl apply -f count_objs.yaml -n <namespace-name>
```


# 2. ConfigMaps
- It is a way to keep configuration settings seperate from the code.
- It is different for each environment.
- We can set and get the values from configmaps.

![configmap](https://matthewpalmer.net/kubernetes-app-developer/articles/configmap-diagram.gif)

- We can easily link these configmaps with the pods.
- For more details https://kubernetes.io/docs/concepts/configuration/configmap/ 

- It is generally used to store the non-crediential information like DATABASE_URI, DATABASE_NAME etc.
- Reference https://matthewpalmer.net/kubernetes-app-developer/articles/ultimate-configmap-guide-kubernetes.html

### How to create configmap file

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  # property-like keys; each key maps to a simple value
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"

  # file-like keys
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5    
  user-interface.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true  
```

### How to Use Configmap

```yaml
apiVersion: v1

kind: Pod

metadata:

  name: configmap-demo-pod

spec:

  containers:

    - name: demo

      image: alpine

      command: ["sleep", "3600"]

      env:

        # Define the environment variable

        - name: PLAYER_INITIAL_LIVES # Notice that the case is different here

                                     # from the key name in the ConfigMap.

          valueFrom:

            configMapKeyRef:

              name: game-demo           # The ConfigMap this value comes from.

              key: player_initial_lives # The key to fetch.

        - name: UI_PROPERTIES_FILE_NAME

          valueFrom:

            configMapKeyRef:

              name: game-demo

              key: ui_properties_file_name

      volumeMounts:

      - name: config

        mountPath: "/config"

        readOnly: true

  volumes:

  # You set volumes at the Pod level, then mount them into containers inside that Pod

  - name: config

    configMap:

      # Provide the name of the ConfigMap you want to mount.

      name: game-demo

      # An array of keys from the ConfigMap to create as files

      items:

      - key: "game.properties"

        path: "game.properties"

      - key: "user-interface.properties"

        path: "user-interface.properties"

        
```

#### How to check Configmaps
`kubectl get configmaps game-demo -o yaml`
`kubectl describe configmaps game-demo`


# 3. Secrets
- Used to Store confidential information.
- It an encripted form of confidential data.
- we can store like `app_key`,`app_secret`,`password` etc.
- For More details https://kubernetes.io/docs/concepts/configuration/secret/

```yaml
# mysecrets.yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm

```
### How to create and check Secrets

```cmd
kubectl apply -f mysecrets.yaml
kubectl get secrets
kubectl get secrets mysecret
```

### Use Secrets as env Variables in Pods
1. As Environment Variables

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
  - name: mycontainer
    image: redis
    env:
      - name: SECRET_USERNAME
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: username
            optional: false # same as default; "mysecret" must exist
                            # and include a key named "username"
      - name: SECRET_PASSWORD
        valueFrom:
          secretKeyRef:
            name: mysecret
            key: password
            optional: false # same as default; "mysecret" must exist
                            # and include a key named "password"
  restartPolicy: Never
```
# 4. Pods [PODS](https://github.com/sharmatriloknath/Docker-Kubernetes-For-Data-Science/blob/main/Pods.md)
# 5. Deployment [DEPLOYMENT](https://github.com/sharmatriloknath/Docker-Kubernetes-For-Data-Science/blob/main/Deployments.md)
# 6. Service [SERVICE](https://github.com/sharmatriloknath/Docker-Kubernetes-For-Data-Science/blob/main/Service.md)

# Deployments

## Labels and Selectors
> Deployments created with 'kubectl create deploy' automatically get the label app=deploymentname
> Pods started with 'kubectl run' automatically get the label run=podname
> The purpose of the label is to connect different objects
> - Deployments tracking Pods
> - Services connecting Pods
> The selector is used on resources to specify which label to track
> As a command line argument --selector can be used to filter output on the precence of a label

```bash
kubectl -n temp create deploy webapp --image=nginx --replicas=3

kubectl -n temp get deployment webapp -o yaml

kubectl -n temp get all --selector app=webapp

kubectl -n temp get all --show-labels
```

### Managing Labels
#### Manually Setting labels
```bash
kubectl label
```

#### Remove label - by using labelname and postfixed with a dash
> remove label app with withs value
```bash
kubectl label app-
```

> **Example** 
```bash
kubectl create deploy bluelabel --image=nginx -n temp

kubectl label deployment bluelabel state=demo -n temp

kubectl -n temp get deployments --show-labels

kubectl -n temp get all --selector app=bluelabel

# as a example, remove the app label from one of the pods
kubectl -n label pod bluelabel-5dd-xxx app-
```

## Annotations
>
```bash
kubectl -n temp create deploy notes --image=nginx --dry-run=client -o yaml > notes.yaml

kubectl apply -f notes.yaml 

kubectl -n temp describe deployments.apps notes

kubectl -n temp annotate deploy notes newnote="demo note"

kubectl -n temp describe deployments.apps notes
```


## Deployment Scaling
### To manually manage application scalability (without HPA) use the scale command
```bash
kubectl create deploy scaleapp --image=nginx --replicas=2
kubectl get all
kubectl scale deploy scaleapp --replicas=3
kubectl get all
```

## Managing Rolling Updates
> To manage how applications are updated, an update stratergy is used
> - strategy.type.rollingUpdate (default) updates in batches to ensure app functionality continues to be offered at any time
> - strategy.type.recreate - brings down all instances, after which new app version is brought up

> To manage rolling updates, 2 parameters are used
> - maxSurge specifies how many applciation instance can be running during the update above the regualr number of application instances
> - maxUnavailable defines how many application instances can be temporariliy unavailable

> To change app / image versions, you can use the Kubectl set image command
```bash
kubectl set image deploy/myapp containername=new_image
```
> **Example**
```bash
kubectl create deployment myapp --image nginx:1.17 --replicas 3

kubectl get deployments myapp -o yaml | grep -A5 strategy

# remeber you are specifing the container name against the image
kubectl set image deployments myapp nginx=nginx:1.18
```


# edit manifest for changing the strategy type

```bash
kubectl edit deploy myapp
```
> old config
```yaml
spec:
  progressDeadlineSeconds: 600
  replicas: 3
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: myapp
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
```

> new config
```yaml
spec:
  progressDeadlineSeconds: 600
  replicas: 3
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: myapp
  strategy:
    type: Recreate
```

```bash
kubectl get deployments myapp -o yaml | grep -A5 strategy
```

## Deployment History
> During the deployment update, the deployments creates a new ReplicaSet that uses the new properties
> The old ReplicaSet is kept, but num of Pods will be set to zero
> This makes it easy to rollback to previous state
> Kubectl rollout history, will show the rollout history

> **Example**
```bash
kubectl create deployment rolling --image nginx:1.14 --dry-run=client -o yaml > rolling.yaml

kubectl apply -f rolling.yaml

kubectl rollout history deployment 
#deployment.apps/rolling 
#REVISION  CHANGE-CAUSE
#1         <none>
kubectl edit deployment rolling  # change to 1.15
#deployment.apps/rolling 
#REVISION  CHANGE-CAUSE
#1         <none>
#2         <none>
kubectl rollout history deployment 

# To see the details of the different revisions
kubectl rollout history deployment rolling --revision 1
# deployment.apps/rolling with revision #1
# Pod Template:
#   Labels:       app=rolling
#         pod-template-hash=7c5d8dcbb9
#   Containers:
#    nginx:
#     Image:      nginx:1.14
#     Port:       <none>
#     Host Port:  <none>
#     Environment:        <none>
#     Mounts:     <none>
#   Volumes:      <none>
#   Node-Selectors:       <none>
#   Tolerations:  <none>

kubectl rollout history deployment rolling --revision 2
# deployment.apps/rolling with revision #2
# Pod Template:
#   Labels:       app=rolling
#         pod-template-hash=7585f65884
#   Containers:
#    nginx:
#     Image:      nginx:1.15
#     Port:       <none>
#     Host Port:  <none>
#     Environment:        <none>
#     Mounts:     <none>
#   Volumes:      <none>
#   Node-Selectors:       <none>
#   Tolerations:  <none>


# Revert to a specific revision
kubectl rollout undo deployment rolling --to-revision 1
kubectl describe deployments rolling 
```

## StatefulSets
> StatefulSet maintains the identity of Pods, even if restarted
> Required by stateful applications like DBs
> Useful when one fo below is required
> - stable, unique network identifiers
> - stable, persistent storage
> - Ordered graceful deployment and scaling
> - Ordered and automated rolling updates
> StatefulSet volumeClaimTemplate generates a new PVC that connects to a PV
[Example to follow along with](https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/)

## DaemonSet
> DaemonSet is a deployment that starts one pod instance on each node in cluster
> useful when you need a agent on every node
> when nodes are added or removed, DaemonSet automatically changes the number of Pods accordingly
> Use YAML to create DaemonSets
[DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)

> **Note**, When using YAML to create the set, you can modify a normal Deployment
You need to change
- kind to DaemonSet
- remove ref to replicas
- remove strategy
so it would look like the below
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  creationTimestamp: null
  labels:
    app: mydaemonset
  name: mydaemonset
spec:
  selector:
    matchLabels:
      app: mydaemonset
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: mydaemonset
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

```bash
kubectl get ds,pods
```
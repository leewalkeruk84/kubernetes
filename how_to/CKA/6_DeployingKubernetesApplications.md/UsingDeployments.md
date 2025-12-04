# Using Deployments

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
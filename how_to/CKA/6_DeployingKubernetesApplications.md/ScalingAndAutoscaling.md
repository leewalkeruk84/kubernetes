# Scaling Applications
`kubectl scale` is used to manually scale Deployment, ReplicaSet, or StatefulSet

```bash
kubectl scale deployment myapp --replicas=3
```

Alternativly, HorizontalPodAutoscaler can be used. Using `kubectl autoscale`, referencing a existing resource

# Autoscaling
HorizontalPodAutoscaler (HPA) is an API resource that manages autoscaling

It works on usage statistics that have been gathered by the metrics server

If configured, it will add Pod instances after a specific threshold has been passed

When observered value drops below the threshold, after a preiod of 5 minutes, the application will be scaled down. There are two ways to change this behaviour:
- As a system default, using kube-controller-manager parameter
- As a specific HPA setting, using, `spec.behavior.scaleDown,stabilizationWindowSeconds`
**Note** Remeber to use explain, to see field usage `kubectl explain hpa.spec.behavior`


```bash
kubectl autoscale -h | less

Examples:
  # Auto scale a deployment "foo", with the number of pods between 2 and 10, no target CPU utilization specified so a default autoscaling policy will be used
  kubectl autoscale deployment foo --min=2 --max=10
  
  # Auto scale a replication controller "foo", with the number of pods between 1 and 5, target CPU utilization at 80%
  kubectl autoscale rc foo --max=5 --cpu=80%
  
  # Auto scale a deployment "bar", with the number of pods between 3 and 6, target average CPU of 500m and memory of 200Mi
  kubectl autoscale deployment bar --min=3 --max=6 --cpu=500m --memory=200Mi
  
  # Auto scale a deployment "bar", with the number of pods between 2 and 8, target CPU utilization 60% and memory utilization 70%
  kubectl autoscale deployment bar --min=3 --max=6 --cpu=60% --memory=70%
```

To change scale down behaviour
```bash
kubectl explain hpa.spec.behavior
kubectl explain hpa.spec.behavior.scaleDown

kubectl edit hpa ...

# add the following, to change to 30 seconds
spec:
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 30

# view new behavior in describe command output
kubectl describe hpa ...
```
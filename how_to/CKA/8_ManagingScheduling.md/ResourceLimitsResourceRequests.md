# Configuring Resource Limits and Requests

## Understanding LimitRange

LimitRange is an API obect that limits resource usage per container or Pod in a Namespace

It uses 3 relevant options:
- `type`: specifies whether it applies to Pods or containers
- `defaultRequest`: the default resources the application will request
- `default`: the maximim resources the application can use

## Understanding Quota
Quota is a API object that limits resources available in a Namespace

If a Namespace is configured with Quota, applications in that Namespace must be configured with Resource settings in `pod.spec.containers.resources`

## LimitRange vs Quota
Where the goal of `LimitRange` is to set default restrictions for each application running in a Namespace, the goal of a `Quota` is to define maximum resources that can be consumed within a Namespace by all applications.

## Setting Quota

```bash
kubectl create ns limited
# Create Quota
kubectl create quota qtest --hard pods=3,cpu=100m,memory=500Mi --namespace limited 

kubectl -n limited describe quota qtest 
# create Deployment without setting limits or requests
kubectl -n limited create deployment nginx --image=nginx:latest --replicas=3

kubectl get all -n limited 

kubectl -n limited describe rs/nginx-7c5d8bf9f7 
# Error showing that requests or limits not set
Error creating: pods "nginx-7c5d8bf9f7-vj62v" is forbidden: failed quota: qtest: must specify cpu for: nginx; memory for: nginx

# set the requests and Limits for deplyoment
kubectl set resources deploy nginx --requests cpu=100m,memory=5Mi --limits cpu=200m,memory=20Mi --namespace limited 

# see that Pods can now start up
kubectl get all -n limited 
```

## Configuring LimitRange
```bash
# limitrange.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```

```bash
kubectl explain limitrange.spec

kubectl create ns limitrange

kubectl apply -f limitrange.yaml -n limitrange

kubectl describe limitrange -n limitrange

kubectl run limitpod --image=nginx -n limitrange

kubectl describe pod limitpod -n limitrange
```
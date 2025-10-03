# Resource Requests, Limits, and Quotas

> - Resource requests can be set for contianers in a Pod to ensure that the Pod is only scheduled on cluster nodes that meet the rescource requests (minimum)
> - Resource Limits can be set for Pods to maximise the use of system resources (maximum)
> - Quota are restrictions that can be set on a Namespace to maximise the availablity of resources within a Namespace

```bash
# to apply resource limits to running applications in deployments
kubectl set resources -h | less 

# Set a deployments nginx container cpu limits to "200m" and memory to "512Mi"
kubectl set resources deployment nginx -c=nginx --limits=cpu=200m,memory=512Mi
  
# Set the resource request and limits for all containers in nginx
kubectl set resources deployment nginx --limits=cpu=200m,memory=512Mi --requests=cpu=100m,memory=256Mi
  
# Remove the resource requests for resources on containers in nginx
kubectl set resources deployment nginx --limits=cpu=0,memory=0 --requests=cpu=0,memory=0
```

## Quota
```bash
kubectl create quota -h | less
```
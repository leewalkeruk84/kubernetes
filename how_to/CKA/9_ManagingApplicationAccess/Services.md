# Services

## Creating Services

> 'kubectl expose' is the most important command to create Services, providing access to Deployments, ReplicaSets, Pods or other Services
> When creating a Service, the '--port' argument must be specified to indicate the port on which the Service will be lsitening for incoming traffic
> While working with Services different ports are specified
> - targetPort : the port on the application (container) that the services addresses
> - port: the port on which the service is accessible
> - nodePort: the port that is exposed externally while using the NodePort Service

> **Examples with Services**
```bash
# Create a deployment
kubectl create deployment nginxsvc --image=nginx
# Scale deployment
kubectl scale deployment nginxsvc --replicas=2
# Create the service (by exposing deployment)
kubectl expose deployment nginxsvc --port=80

kubectl describe svc nginxsvc # look for endpoints
kubectl get svc nginxsvc -o yaml
kubectl get svc 
kubectl get endpoints
```

```bash
# Create a deployment
kubectl create deployment nginx --image=nginx --port=8080
# Scale deployment
kubectl scale deployment nginx --replicas=3
# Create the service (by exposing deployment)
kubectl expose deployment nginx --port=80 --target-port=8080 --name=nginxservice


kubectl describe svc nginxservice # look for endpoints
kubectl get svc nginxservice -o yaml
kubectl get svc 
kubectl get endpoints
```

### NodePort
```bash
kubectl create deployment nodeportdeploy --image=nginx
kubectl expose deployment nodeportdeploy --port=80 --name=nodeportsvc --type=NodePort
kubectl get svc nodeportsvc -o yaml
kubectl get endpoints
```

### Headless service
```bash
kubectl create deployment headless --image=nginx

kubectl expose deployment headless --port=80 --name=headlesssrv --cluster-ip=None

kubectl get svc headlesssvc -o yaml
kubectl get endpoints
```
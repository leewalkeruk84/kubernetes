# Blue Green

> **Notes**
> If you have a Blue and Green app, you would test that the new app is working
> You would change the label selector on the Ingress or Service to the new application
> You would test that it is hitting your new app
> You would then scale down the old app to replicas=0, or delete it
```bash
kubectl scale deploy blue-deploy --replicas=0
```

## Example
```bash
kubectl create deployment blue-nginx --image=nginx:1.16 --replicas=3

kubectl create deployment green-nginx --image=nginx:1.17 --replicas=3 

kubectl expose deployment blue-nginx --port 80 --name=bgnginx

kubectl get pods -w

kubectl edit service bgnginx
# swap labels to green from blue

# or you could delete the old service and create a new service
kubectl delete svc blue-nginx; kubectl expose deployment green-nginx --port 80 --name=bgnginx

# test
kubectl get service bgnginx -o yaml

# scale down blue or delete deployment
kubectl scale deployment blue-nginx --replicas 0
kubectl get pods -w
kubectl delete deployments.apps blue-nginx 
```
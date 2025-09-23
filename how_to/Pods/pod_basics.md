# Pod Basics 
```bash
kubectl explain pod | less
kubectl explain pod.spec | less
```
## You can then dig into the sub features using dot notation
```bash
kubectl explain pod.metadata | less
kubectl explain pod.spec | less
kubectl explain pod.spec.containers | less
```

# Basics
## Run a stand alone pod via commandline
```bash
kubectl run podname --image=imagename
kubectl run web --image=nginx
kubectl -n dev run web --image=nginx
```

## Run with Environemnt variables
```bash
kubectl run -h | less
```
> Check the examples in the help file
> see the Env vars example
> kubectl run hazelcast --image=hazelcast/hazelcast --env="DNS_DOMAIN=cluster" --env="POD_NAMESPACE=default
> Alter as needed
```bash
kubectl -n secret run mysecretpod --image=nginx --env="USERNAME=user1" --env="PASSWORD=pdw123"

kubectl -n secret run mysecretpod --image=nginx --env="USERNAME=user1" --env="PASSWORD=pdw123" --dry-run=client -o yaml > secretpod.yaml
kubectl -n secret get pod mysecretpod
kubectl -n secret exec -i -t mysecretpod -- env
```

## Run a stand alone pod via YAML config
### Creates resources, if the resources already exists it fails
```bash
kubectl create -f resource.yaml
kubectl -n namespaceName create -f resource.yaml
```


### Creates resources, if ther esource already exists, it updates the properties that need updating - more useful to use this comand that create
```bash
kubectl apply -f resource.yaml
kubectl -n namespaceName apply -f resource.yaml
```

## Generating YAML files
### use the below as a argument to kubectl run or create commands
```bash
--dry-run=client -o yaml > my.yaml
```
```bash
kubectl -n dev run mynginx --image=nginx --dry-run=client -o yaml > mynginx.yaml
kubectl apply -f mynginx.yaml
```

## Multi-container Pods
### Sidecar container
> container that enhances the primary application, for instance logging, monitoring, syncing

### Ambassador container
> container that represents the primary container to the outside world, such as a proxy

### Adaptor container 
> container used to adopt the traffic or data pattern to match the traffic or data pattern in other applications in the cluster


## Troubleshooting
```bash
kubectl get pods
kubectl describe pod podname
kubectl logs podname
```
> shell to pod - only useful if the pod is in a running state
```bash
kubectl exec -it podname sh
```
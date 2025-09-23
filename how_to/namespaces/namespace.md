# Namespaces

## Show resources in all namepsaces
```bash
kubectl get enter_resource_type -A
kubectl get pods -A
```

## Show resources in all namepsaces
```bash
kubectl get enter_resource_type -A
kubectl get pods -A
```
## Create namespace
```bash
kubectl create ns nameofnamespace
```
## Run resources in a specific Namespace
```bash
kubectl -n nsname run .....
```
## 
> Example
```bash
kubectl get pods -A
kubectl create ns secret
kubectl -n secret run secretpod --image=nginx
kubectl -n secret get pod secretpod
```
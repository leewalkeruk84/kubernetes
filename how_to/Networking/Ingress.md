# Ingress
>**Note Use either Ingress or GatewayAPI, not both at the same time**

> An Ingress Controller is required to use Ingress Resources
> It receives external traffic and uses the Ingress Resource to route it appropriately.

# Ingress Controller
## Setup on Minikube
```bash
# show available addons
minikube addons list

# enable a specific addon for Ingress
minikube addons enable ingress

kubectl get ns

kubectl get all -n ingress-nginx
```

> To disable the ingress addon if needed (or you want to use GatewayApi which would need its own controller installed)
```bash
minikube addons disable ingress-nginx
```

## Using Ingress
> **Example** : Configuring Ingress Rules
```bash
# create and expose a nginx deployment, called nginxsvc
kubectl create deploy nginxsvc --image nginx --port 80
kubectl expose deploy nginxsvc --port=80

# create a Ingress rule
kubectl create ingress -h | less
# edit most appropriate example
# kubectl create ingress simple --rule="foo.com/bar=svc1:8080
kubectl create ingress nginxsvc-ingress --rule="/=nginxsvc:80"

kubectl describe ingress nginxsvc-ingress

# non exam step
vim /etc/hosts
192.168.49.2    nginxsvc.info
# test
curl nginxsvc.info

# remove the entry in the hosts file
```

## Ingress Paths
> pathType specifies how to deal with path requests
> - Exact pathType indicates that an exact match should occur
> - IE. path set to /foo, only /foo will match, /foo/ will not
> - Prefix pathtype indicates that the requested path should start with
> - IE. path is set to /, any requested path will match. 
> - IE. path set to /foo, both /foo and /foo/ will match

By default, pathType is set to Exact when creating resourcce with kubectl create ingress, if you need prefix, use kubectl edit ingress command to change to Prefix
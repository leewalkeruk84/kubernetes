# Ingress

## Running an Ingress Controller
Ingress is an API object that manages external access to services in a cluster

Ingress works with external DNS to provide URL based access to Kubernetes applications

Ingress consists of 2 parts:
- A physical load balancer available on the external network
- An API resource that contains rules to contact the Service resources to find out about available back-end Pods

Ingress load balancers are provided by the Kubernetes ecosystem, different ones are available

Ingress exposes HTTP and HTTPS routes from outside the cluster to Services within the cluster

Ingress uses the `selectorlabel` in Services to connect to the Pod endpoints

Traffic routing is controlled by rules defined on the Ingress resource

Ingress can be configured to do the following, according to the functionality provided by the load balancer
- Gives Services externally reachable URLs
- Load balance traffic
- Terminate SSL/TLS
- Offer name based virtual hosting

### Demo: Installing the Nginx Ingress Controller

```bash
# https://artifacthub.io/packages/helm/ingress-nginx/ingress-nginx

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm install ingress-nginx ingress-nginx/ingress-nginx --version 4.14.1 --namespace ingress-nginx --create-namespace

kubectl -n ingress-nginx get pods

kubectl -n ingress-nginx create deploy nginxsvc --image=nginx:latest --port=80 

kubectl -n ingress-nginx expose deployment nginxsvc

kubectl -n ingress-nginx describe svc nginxsvc 

kubectl create ingress -h | less
# kubectl create ingress catch-all --class=otheringress --rule="/path=svc:port"

# create Ingress Rule
# anything that comes in on `nginxsvc.info/*` fwd to nginxsvc on port 80
kubectl create ingress nginxsvc --class=nginx --rule="nginxsvc.info/*=nginxsvc:80"

# some steps to make it work locally
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:80
echo "127.0.0.1 nginxsvc.info" /etc/hosts
curl nginxsvc.info:8080
```

## Configuring Ingress
### Managing Rules
Ingress rules catch incoming traffic that matches a specific path and optional hostname and connects that to a Service and a port

Use `kubectl create ingress` to create rules

Different paths can be defined on the same host
`kubectl create ingress myingress --rule="/myingress=myingress:80" --rule="/youringress=youringress:80"`

Different virtual hosts can be defined in the same Ingress
`kubectl create ingress nginxsvc --class=nginx --rule=nginxsvc.info/*=nginxsvc:80 --rule=otherserver.org/*=otherserver.org:80`

### Understanding IngressClass
In one cluster, different Ingress controllers can be hosted, each with its own configuration

Controllers can be included in an `IngressClass`

While defining Ingress rules, the `--class` option should be used to implement the role on a specific Ingress controller
- If this option is not used, a default `IngressClass` must be defined
- Set `ingressclass.kubernetes.io/is-default-class:true` as an annotation on the IngressClass to make it the default
- After creating the Ingress controller as described before, an IngressClass API resource has been created
- Use `kubectl get ingressclass -o yaml` to investigate its contents
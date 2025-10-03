# Authentication and Authorization

## Authentication
Authentication is about where users come from
> The 'kubectl config' specifies to which cluster to authenticate to
> ```kubectl config view``` to see current settings
> File is read from `~/.kube/config

## Authorization
Authorization is about what users can do
> Behind Authorization is RBAC to take care of different options
> To find out what you can do, use kubectl auth can-i:
> ```kubectl auth can-i -h | less ```
> ```kubectl auth can-i create pods ```
> ```kubectl auth can-i get pods -- as=system:serviceaccount:bellview:viewer -n bellview``` # user is viewer, namespace is bellview

> ***Demo***
> ```bash
> kubectl auth can-i get pods
> # yes
> kubectl auth can-i get pods --as bob@example.com
> # no
>```

## API Access and ServiceAccounts
### Understanding ServiceAccounts
> All actions in a cluster need to be authenticated and authorized
> ServiceAccounts are used for basic authentication from within the Kubernetes cluster
> RBAC is used to connect a ServiceAccount to a specific Role
> Every Pod uses the default ServiceAccount to contact the API server
> This default ServiceAccount allows a resource to get information from the API server, but not much else
> Each ServiceAccount uses a Secret to automount API credentials

### CustomServiceAccount Use Case
> Most Pods do fine with default SA
> If a Pod needs access to resources in the cluster, a customer SA that uses a RoleBinding to connect to a specific Role is needed

Exploring Service Accounts
```bash
kubectl get sa 
kubectl describe pod # look for SA account near top of Output

kubectl get sa -n kube-system 
kubectl -n kube-system describe pod coredns # look for SA account near top of Output
```

### Security Context
Follow these Demo's
[Security Context Demo](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)
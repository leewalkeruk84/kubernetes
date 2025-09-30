# Network Policy

> If in a policy there is no match, traffic will be denied
> If no network policy is used, all traffic is allowed

## Network Policy Identifiers
> In Network Policy, 3 different identifiers can be used
> - podSelector: specifies a label to match Pods
> - namespaceSelector: used to grant access to specific namespaces
> - ipBlock: Marks a range of IP addresses that is allowed
> When defining a Pod or Namespace based NetworkPolicy, a selector label is used to specify what traffic is allowed to and from the Pobs that match the selector

> **Important** Namespace selector is for CKA
> [Example to follow along with](https://kubernetes.io/docs/tasks/administer-cluster/declare-network-policy/)

> **Important for the exam**
>
> 
```bash
# to give yourself a simple description of what a policy is doing
kubectl describe networkpolicy <policyname>
```

> **Example**
### Create an nginx deployment and expose it via a service
```bash
# Create an nginx deployment
kubectl create deployment nginx --image=nginx
# Expose the Deployment through a Service called nginx
kubectl expose deployment nginx --port=80

kubectl get svc,pod

kubectl describe svc nginx # output below
```

```yaml
Name:                     nginx
Namespace:                default
Labels:                   app=nginx
Annotations:              <none>
Selector:                 app=nginx
Type:                     ClusterIP
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.99.40.242
IPs:                      10.99.40.242
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
Endpoints:                10.244.120.81:80
Session Affinity:         None
Internal Traffic Policy:  Cluster
Events:                   <none>
```
### Test access
```bash
# you should be able to access from another pod in same namespace
kubectl run busybox --rm -ti --image=busybox -- /bin/sh
# wget --spider --timeout=1 nginx
Connecting to nginx (10.99.40.242:80)
remote file exists
```

### Limit access to the nginx service
To limit the access to the nginx service so that only Pods with the label access: true can query it, create a NetworkPolicy object as follows

> This policy is being applied to Pods with the selector 'app: nginx' and is only allowing incoming traffic to those Pods, if the Pod that wants to talk to it, has the label 'access: "true"
> All nginx Pods (app: nginx) will now deny all ingress traffic except from Pods with access: "true"
> Since egress section isnt specified, the selected Pods (app: nginx) can still send traffic out to anywhere
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: access-nginx
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
  - from:
    - podSelector:
        matchLabels:
          access: "true"
```

```bash
# you should now be blocked access from another pod in same namespace
kubectl run busybox --rm -ti --image=busybox -- /bin/sh
# wget --spider --timeout=1 nginx
Connecting to nginx (10.99.40.242:80)
wget: download timed out

# unless the Pod has a label 'access: true'
kubectl run busybox --rm -ti --image=busybox --labels="access=true"  -- /bin/sh
Connecting to nginx (10.99.40.242:80)
remote file exists
```

> **Important for the exam**
> Check what a NetworkPolicy is doing - using kubectl describe NetworkPolicy
> If a Pod cant access a specific workload, check the traffic initiator has the correct label set
> You can use kubectl label to apply the correct label to the Pod

```bash
# to give yourself a simple description of what a policy is doing
kubectl describe networkpolicy <policyname>
```
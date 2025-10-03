# RBAC

> **Exam Info**: For CKAD you dont have to create Roles or RoleBindings, but you should be able to work with ServiceAccounts
If you find a Pod that isnt running

```bash
# find the service accounts in the namespace you app is running
kubectl get sa -n namespacename
# Set the SA account to the Pod
kubectl set serviceaccount -h | less
# Set deployment nginx-deployment's service account to serviceaccount1
# kubectl set serviceaccount deployment nginx-deployment serviceaccount1
kubectl set serviceaccount deployment nginx-deployment serviceaccount1 -n namespace
```


## Understanding RBAC
> RBAC uses 3 components to grant permissions to API objects
> - Role consists of Verbs which assign specific permissions like view, edit and more
> - ServiceAccount is used by Pods that need access to API resources
> - RoleBinding connects a ServiceAccount to a Role

> Role and RoleBindings have a Namespaced scope, ClusterRoles and ClusterRoleBindings have a cluster scope
> In RBAC users can be used for people that need access to specific resources (not covered in CKAD)

> **Demo**: Configuring RBAC
```bash
kubectl create ns bellevue

kubectl create role -h | less
# Create a role named "pod-reader" that allows user to perform "get", "watch" and "list" on pods
# kubectl create role pod-reader --verb=get --verb=list --verb=watch --resource=pods
kubectl -n bellevue create role viewer --verb get --verb list --verb watch --resource=pods
kubectl -n bellevue describe role viewer

kubectl create sa -h | less
# Create a new service account named my-service-account
# kubectl create serviceaccount my-service-account
kubectl create sa viewer -n bellevue
kubectl -n bellevue describe sa viewer

kubectl create rolebinding -h | less
# Create a role binding for service account monitoring:sa-dev using the admin role
# kubectl create rolebinding admin-binding --role=admin --serviceaccount=monitoring:sa-dev
kubectl -n bellevue create rolebinding viewer --role=viewer --serviceaccount=bellevue:viewer
kubectl -n bellevue describe rolebinding viewer

kubectl create deployment viewnginx --image=nginx --replicas 3 -n bellevue
kubectl set serviceaccount -h | less
# Set deployment nginx-deployment's service account to serviceaccount1
# kubectl set serviceaccount deployment nginx-deployment serviceaccount1
kubectl set serviceaccount deployment viewnginx viewer -n bellevue 

kubectl auth can-i get -h | less
# Check to see if service account "foo" of namespace "dev" can list pods in the namespace "prod"
# You must be allowed to use impersonation for the global option "--as"
# kubectl auth can-i list pods --as=system:serviceaccount:dev:foo -n prod
kubectl auth can-i list pods --as=system:serviceaccount:bellevue:viewer -n bellevue

```
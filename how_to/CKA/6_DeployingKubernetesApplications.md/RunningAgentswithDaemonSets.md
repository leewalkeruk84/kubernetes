# Running Agents with DaemonSets

A DaemonSet is a resource that starts one application instance on each cluster node

It is commonly used to start agents like the kube-proxy that need to be running on all cluster nodes

It can be used for user workloads

If the DaemonSet needs to run on control-plane nodes, a toleration must be configured to allow the pods to run regardless of the control-plane taints

# Quickyl Create a Simple DaemonSet
- Create a deployment yaml manifest using `kubectl create deploy`
- Change kinf from `Deployment` to `DaemonSet`
- Remove `strategy` and `replicas` as DaemonSets dont have them

```bash
kubectl create deploy daemon --image=nginx --dry-run=client -o yaml > daemon.yaml
```

```bash
apiVersion: apps/v1
kind: Deployment    # Change to DaemonSet
metadata:
  labels:
    app: daemon
  name: daemon
spec:
  replicas: 1       # Delete line
  selector:
    matchLabels:
      app: daemon
  strategy: {}      # Delete line
  template:
    metadata:
      labels:
        app: daemon
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```
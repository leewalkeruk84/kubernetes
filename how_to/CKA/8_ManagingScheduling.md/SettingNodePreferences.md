# Setting Node Preferences

The `nodeSelector` field in the `pod.spec` specifies a key-value pair that must match a label which is set on nodes that are eligible to run the Pod

Use `kubectl label nodes worker1 disktype=ssd` to set a label on a node

Then, use `nodeSelector: disktype: ssd` in the `pod.spec` to match the Pod to the specific node

`nodeName` is the part of the `pod.spec` and can be used to always run a Pod on a node with a specific name

## Example
```bash
kubectl get nodes --show-labels 

kubectl label nodes w1 disktype=ssd

kubectl get nodes --show-labels 

# yaml file for a Pod with the nodeSelector set
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: ssdpod
  name: ssdpod
spec:
  nodeSelector: 
    disktype: ssd
  containers:
  - image: nginx
    name: ssdpod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

# yaml file for a Deployment with the nodeSelector set
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: ssddeploy
  name: ssddeploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ssddeploy
  strategy: {}
  template:
    metadata:
      labels:
        app: ssddeploy
    spec:
      nodeSelector:
        disktype: ssd
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}

# in both examples, the Pod will run on node(s) with the 'disktype: ssd' label on the node
```

```bash
# Example if wanted to remove the label from the node
kubectl label node w1 disktype-
```
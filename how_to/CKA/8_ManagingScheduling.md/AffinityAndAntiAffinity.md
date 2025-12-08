# Managing Affinity and anti-Affinity Rules

## Understanding Affinity and Anit-Affinity

Affinity and Anit-Affinity is used to define advanced scheduler rules

Node affinity is used to constrain a node that can recieve a Pod by matching labels of these Nodes

Inter-Pod affinity contrains nodes to recieve Pods by matching labels of existing Pods already running on that Node

Anti-affinity can only be applied between Pods

## How It Works

A Pod that has a `nodeAffinity` label of `key=value` will only be scheduled to a node with a matching label 

A Pod that has `podAffinity` label of `key=value` will only be scheduled to nodes running Pods with the matching label. i.e. to group pods together

## Setting Node Affinity
[NodeAffinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#node-affinity)
To define node affinity, two different statements can be used:
- `requiredDuringSchedulingIgnoredDuringExecution`: The scheduler can't schedule the Pod unless the rule is met.
- `preferredDuringSchedulingIgnoredDuringExecution`: The scheduler tries to find a node that meets the rule. If a matching node is not available, the scheduler still schedules the Pod
- While using `preferredDuringSchedulingIgnoredDuringExecution`, a weight can be assigned to affinities to raise or lower priorities

```bash
apiVersion: v1
kind: Pod
metadata:
  name: with-node-affinity
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In
            values:
            - antarctica-east1
            - antarctica-west1
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: another-node-label-key
            operator: In
            values:
            - another-node-label-value
  containers:
  - name: with-node-affinity
    image: registry.k8s.io/pause:3.8
```
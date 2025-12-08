# Managing Taints and Tolerations

Taints are applied to a node to mark that the node should not accept any Pod that doesn't tolerate the taint

Tolerations are applied to Pods and allow (but do not require) Pods to schedule on nodes with matching Taints - so they are an exception to taints that are applied

Where Affinities are used on Pods to attract them to specific nodes, Taints allow a node to repel a set of Pods

Taints and Tolerations are used to ensure Pods are not scheduled on inappropriate nodes, and thus make sure that dedicated nodes can be configured for dedicated tasks

## Understanding Taint Types

3 types of Taint can be applied
- `NoSchedule`: does not schedule new Pods
- `PreferNoSchedule`: does not schedule new Pods, unless there is no other option
- `NoExecute`: migrate all Pods away from this node
If the Pod has a toleration however, it will ignore the taint

## Setting Taints
Taints are set in different ways

Control Plane nodes automatically get taints that won't schedule user Pods

When `kubectl drain` and `kubectl cordon` are used, a taint is applied on the target node

Taints can be set automatically by the cluster when critical conditions arise, such as a node running out of disk space

Admins can use `kubectl taint` to set taints:
- `kubectl taint nodes worker1 key=value1:NoSchedule` To set
- `kubectl taint nodes worker1 key=value1:NoSchedule-` To Remove

## Understanding Toleration
To allow a Pod to run on a node with a specific taint, a toleration can be used

This is essential for running core Kubernetes Pods on the control plane node

While creating taints and tolerations, a key and a value are defined to allow for more specific access
`kubectl taint nodes worker1 storage=ssd:NoSchedule`

This will allow a Pod to run if it has a toleration containing the key `storage=ssd`

## Understanding Taint Key and Value
While defining a toleration, the Pod needs a key, operator, and a value
```bash
tolerations:
- key: "storage"
  operator: "Equal"
  value: "ssd"
```

The default value for the operator is `Equal`; As an alternative `Exists` is commonly used.

If the operator `Exists` is used, the key should match the taint key, and the value is ignored

If the value `Equal` is used, the key and value must match

## Node Conditions and Taints
Node Conditions can automatically create taints on nodes if one of the following applies
- memory-pressue
- disk-pressure
- pid-pressure
- unschedulable
- network-unavailable

If any of the above conditions apply, a taint is automatically set

Node conditions can be ignored by adding corresponding Pod tollerations, but isnt really a smart move
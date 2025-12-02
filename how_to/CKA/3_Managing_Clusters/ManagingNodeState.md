## Managing Node State
[Node Status](https://kubernetes.io/docs/reference/node/node-status/)

When using `cordon` or `drain`, a taint is set on the nodes
A taint is a restriction that prevents Pods from running or being scheduled on a node

```bash
# to mark a node as unschedulable
kubectl cordon --help | less
```

```bash
# to mark a node as unschedulable and remove all running pods from it
kubectl drain --help | less

# Pods that have been started from a Daemon Set will not be removed, you must add param --ignore-daemonsets

# Add --delete-emptydir-data to delete data from the emptyDir Pod volumes
```

```bash
# to bring a node back into a operational state
kubectl uncordon --help | less
```

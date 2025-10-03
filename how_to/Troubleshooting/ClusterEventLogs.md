# Monitoring Cluster Event Logs

```bash
kubectl get events # overview of cluster wide events

kubectl get events -o wide # overview of cluster wide events with more detail

kubectl describe ... # better to use this on the resource if you know which one is involved
```
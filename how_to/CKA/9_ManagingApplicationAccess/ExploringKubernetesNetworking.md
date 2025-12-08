# Exploring Kubernetes Networking
In kubernetes, networking happens at different levels
- Between containers: implemented as Inter Process Communication (IPC)
- Between Pods: implemented by network Plugins
- Between Pods and Services: Implemented by Service resources
- Between external users and Services: Implemented by Services, with the help of Ingress or Gateway API

For a long time, Ingress has been the solution to manage incoming traffic.

Recently Ingress has gone into feature freeze and will be replaced by Gateway API.

**Check exam objectives, to see which is needed (or both)**
**Warning: Do not configure Ingress and Gateway API on the same machine!**


# Understanding Network Plugins
Network plugins are required to implement network traffic between Pods

Network plugins are provided by the Kubernetes Ecosystem

Vanilla Kubernetes does not come with a default network plugin, and youll have to install it while installing a cluster

Network plugins add resources to the Kubernetes APIs using Custom Resource Definitions

```bash
kubectl get crd

# view yaml representation of a crd from output above
kubectl get ippools.crd.projectcalico.org -A -o yaml

```
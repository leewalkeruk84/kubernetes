# Running Statis Pods
[Static Pods](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/)

## Understanding Static Pods
The kubelet systemd process is configured to run static pods from the `/etc/kubernetes/manifest` directory

On the control node, static Pods are an essential part of how kubernetes works. systemd starts kubelet, and the kubelet starts core kubernetes services as static pods

You can manually add static pods if desired, just copy a manifest file into `/etc/kubernetes/manifest` directory and the kubelet process will pick it up

See the default Static Pods that are setup on the Control Node
```bash
ls -l /etc/kubernetes/manifests/

-rw------- 1 root root 2547 Dec  1 13:44 etcd.yaml
-rw------- 1 root root 3897 Dec  1 13:44 kube-apiserver.yaml
-rw------- 1 root root 3279 Dec  1 13:44 kube-controller-manager.yaml
-rw------- 1 root root 1656 Dec  1 13:44 kube-scheduler.yaml
```

## Use cases
- Static pods are used to start core Kubernetes services
- Can also be used to run agents, and doing so, youll guarantee agent accessibility even if the API server is down
- Useful in cluster recovery scenarios as they can provide services while the API services is down

## Demo
```bash
kubectl run staticpod --image=nginx --dry-run=client -o yaml > staticpod.yaml

kubectl get pods -o wide
sudo cp staticpod.yaml /etc/kubernetes/manifests/
# notice pod has started after kubelet picks up manifest
kubectl get pods -o wide
```
**Note** drop the manifest into the `/etc/kubernetes/manifests/` dir on the node you want the static pod to run 


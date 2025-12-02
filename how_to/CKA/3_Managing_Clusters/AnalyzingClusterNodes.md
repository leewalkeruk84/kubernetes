## Analyzing Cluster Nodes

To monitor nodes processes, standard Linux rules apply
```bash
sudo systemctl status kubelet # to get runtime info about kubelet
```

```bash
kubectl describe node <nodename>  # node info

# if metrics server is installed
kubectl top nodes # to get a summary of CPU/Memory 
```

#### Basic Sanity Checks for a Node called control
```bash
kubectl describe node control | less

ssh control

ls -lrt /var/log

sudo journalctl

systemctl status kubelet

sudo systemctl start kubelet
```
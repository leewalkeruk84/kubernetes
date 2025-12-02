# Setting Up CRI and Tools

These have been run on all 3 nodes.
CRI and previous versions of kubetools
Snaphot - 'Phase 1 Setup'
```bash
cd Downloads/cka
./setup-container.sh
./setup-kubetools-previousversion.sh
```

```bash
WARN[0000] Config "/etc/crictl.yaml" does not exist, trying next: "/usr/bin/crictl.yaml" 
after initializing the control node, follow instructions and use kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml to install the calico plugin (control node only). On the worker nodes, use sudo kubeadm join ... to join
```

## Slow / Unresponsive etcd 
When creating print new join tokens, operation failed due to slow or unresponsive etcd. The fix was to move etcd to tmpfs (RAM)

```bash
# Stop everything safely
sudo systemctl stop kubelet
sudo systemctl stop containerd   # or docker if you use docker

# Move current etcd data to RAM
sudo mv /var/lib/etcd /var/lib/etcd.bak
sudo mkdir /var/lib/etcd
sudo mount -t tmpfs tmpfs /var/lib/etcd
sudo cp -a /var/lib/etcd.bak/* /var/lib/etcd/
sudo chown -R root:root /var/lib/etcd

# Restart
sudo systemctl start containerd   # or docker
sudo systemctl start kubelet
```
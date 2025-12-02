# Setting up Cluster
**Important - kubeadm init is only ran on Control Node** 
**If you dont need custom values, you can just run `kubeadm init` on the control node** 
```bash
sudo kubeadm init
```

## kubeadm with config file
```bash
# view the default values
kubeadm config print --help
kubeadm config print init-defaults
```

```bash
# write config to values file
kubeadm config print init-defaults > config.yaml
```

use vim on the config file to update a default value to a custom value

Then you can use the config file to initialise the control plane node

**Important - kubeadm init is only ran on Control Node** 
```bash
sudo kubeadm init --config config.yaml
```

## Revert kubeadm init
If `kubeadm init` fails, use `kubeadm reset` to  remove components that were installed. It will not remove CRI or Kubetools that were previously installed, only kube components from kubeadm init command

`kubeadm reset` removes a failed cluster initilaization, as well as a failed cluster join

## Output from `kubeadm init` command
Look at the output of the bottom of the `kubeadm init` command for next instructions

**Example Output**
```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.127.131:6443 --token zmqk1k.m3d27frr5n5619qa \
	--discovery-token-ca-cert-hash sha256:e609a0fc1448241cdc3a700ea05b571cba8d351899093866c56fc07284ca008c
```

## Manage Cluster
From the output, you will have to run these commands to operate the cluster
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Setting up Node Networking
**see from the output above**
```
You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/
```

```bash
kubectl get pods -n kube-system
# Run "kubectl apply -f [podnetwork].yaml"
# using the calico script provided in cka folder
kubectl get pods -n kube-system -w
```

## Adding nodes to cluster
Use the join command with sudo privillages from the output of `kubeadm init` command
**Example**
```bash
# on control node
kubectl get nodes

# move to worker node and run
sudo kubeadm join 192.168.127.131:6443 --token zmqk1k.m3d27frr5n5619qa \
	--discovery-token-ca-cert-hash sha256:e609a0fc1448241cdc3a700ea05b571cba8d351899093866c56fc07284ca008c

# on control node
kubectl get nodes
```

### Expired or lost join token
```bash
sudo kubeadmm token create --print-join-command
```
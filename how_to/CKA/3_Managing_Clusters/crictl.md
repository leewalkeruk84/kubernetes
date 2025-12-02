## Using 'crictl' to manage Node Containers

`crictl` is a command-line interface for CRI-compatible container runtimes. You can use it to inspect and debug container runtimes and applications on a Kubernetes node

[crictl](https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md)

[Debugging with crictl](https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/)

`crictl` is a generic tool that communicates to the container runtime to get info about running containers, as such it replaces generic tools like docker and podman

To use it, a runtime-endpoint and image-endpoint must be set. Can do this via command line but easiest way is to edit the `/etc/crictl.yaml` file 

## Demo endpoint
```bash
cat /etc/crictl.yaml
# output
runtime-endpoint: "unix:///run/containerd/containerd.sock"
image-endpoint: ""
timeout: 0
debug: false
pull-image-on-create: false
disable-pull-on-run: false
```
`unix:///run/containerd/containerd.sock` already pointing to container runtime. 

## Demo using crictl

### Help
```bash
crictl --help | less
```

### List images
```bash
sudo crictl images
```

### List all containers
```bash
sudo crictl ps -a
```

### List running containers
```bash
sudo crictl ps
```

### Get container logs
```bash
# use container ID from output of crictl ps command
sudo crictl logs 16085360a3d06
```

### List all pods
```bash
sudo crictl pods
```

### List pods by name
```bash
sudo crictl pods --name etcd-cp1
```

### Execute a command in a running container
```bash
# use container ID from output of crictl ps command
sudo crictl exec -i -t 16085360a3d06 ls
```

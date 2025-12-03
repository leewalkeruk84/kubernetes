# Etcd

## Overview
The Etcd is a core Kubernetes service that contains all the resources that have been created
It is started by the kubelet as a static Pod on the control node
Losing Etcd, means losing all your config

## Understanding Etcd Backup
To backup the etcd, root access is required to run the `etcdctl` tool

Use `sudo apt install etcd-client` to install the tool

To use `etcdctl`, you need to specify the etcd service API endpoint, as well as cacert, cert and key to be used. Values for these can be obtained using `ps aux | grep etcd`

### Demo Backup Etcd
A good starting command from the docuemntation is 
https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#built-in-snapshot
It even gives you what 4 bits of info you need from the decribe command on etcd pod
`where trusted-ca-file, cert-file and key-file can be obtained from the description of the etcd Pod`

```bash
sudo apt install etcd-client

sudo etcd-ctl --help | less
# look for snapshot save command -
# snapshot save           Stores an etcd node backend snapshot to a given file
sudo etcdctl snapshot save --help | less
```

You now need to find the values for cacert, cert and key, plus the port of where is it running
```bash
# you want the following files, ca.crt, server.crt, server.key
# you can get these by describing the etcd pod
# Etcd is deployed as a Pod in the kube-system namespace. The name of the Pod is etcd-controlplane:
kubectl get pods -n kube-system
kubectl describe pod etcd-xxx -n kube-system
```

Look for the value of the option `--listen-client-urls` for the endpoint URL. In the output below, the host is localhost and the port is 2379. 
The `CA certificate` is located at `/etc/kubernetes/pki/etcd/ca.crt` specified by the option `--trusted-ca-file`
The `Server certificate` is located at `/etc/kubernetes/pki/etcd/server.crt` specified by the option `--cert-file` 
The `Server Key` is located at `/etc/kubernetes/pki/etcd/server.key` specified by the option `--key-file`

```bash
ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
--cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```

```bash
# build the command and run it with get /, to test info from the api rather run it blindly
sudo etcdctl --endpoints=localhost:2379 
--cacert /etc/kubernetes/pki/etcd/ca.crt  --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key get / --prefix --keys-only

# if it returns the keys your taking to the etcd, so ou can carry on with out the / get and prefix 

sudo etcdctl --endpoints=localhost:2379 
--cacert /etc/kubernetes/pki/etcd/ca.crt  --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key snapshot save /tmp/etcdsavefile.db
```

Verify the Etcd backup
```bash
sudo etcdctl --write-out=table snapshot status /tmp/etcdsavefile.db
```


## Restore a Etcd Backup
> **Important:** This isnt documented well, you will need to know this off by heart for the exam
Limited info here
https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#upgrading-etcd-clusters

- Stop the core Kubernetes services - temp move
  `mv /etc/kubernetes/manifests/*.yaml /etc/kubernetes`
  As the kubelete process temporarily polls for static Pod fils, the etcd process will disappear within a minute
  use `sudo crictl ps` to verify it has been stopped
- Rename etcd dir: 
  `sudo mv /var/lib/etcd /var/lib/etcd-old`
- Restore the back up
  `sudo etcdctl snapshot restore /backuplocation/file.bak --data-dir /var/lib/etcd`
- Move static fils back
  `mv /etc/kubernetes/*.yaml /etc/kubernetes/manifests`
- Verify Pods have restarted
  `sudo crictl ps`
- Verify and show original etcd resources
  `kubectl get all`
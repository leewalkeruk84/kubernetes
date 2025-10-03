# Troubleshooting Authentication Logs

> Access to cluster is configured through the ~/.kube/config file
> The file is copied from the control node in the cluster, where it is stored as /etc/kubernetes/admin.conf
> Use ```bash kubectl config view``` to check contents of file
> For additional authorization based probelms, use kubectl auth can-i:
> ```bash kubectl auth can-i -h | less ```
> ```bash kubectl auth can-i create pods ```
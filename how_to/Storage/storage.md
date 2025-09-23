# Storage
> **Note**: There is no easy command to create a Pod with Volumes, use the documentation to set it up

> **Documentation search terms and links
> [Configure a Pod to Use a Volume] (https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/)

> [Configure a Pod to Use a PersistentVolume] (https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/)

#### Common Pod Volume types
- emptyDir, create a temp dir on the host that runs a pod and is ephemeral
- hostPath, refers to a persistent dir on the host that runs the Pod
- PVC connects to available PersistentVolumes
- fc and iscsi, make more sense in real life but not covered on CKAD

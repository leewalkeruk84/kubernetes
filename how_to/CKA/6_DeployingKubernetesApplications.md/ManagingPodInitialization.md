# Managing Pod Initialization
[Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)

## Init container
> special container where the init container runs to completion before the main container is started 
> if init contianer fails, main container, will never start
[Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)
[Configure Pod Initialization](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-initialization/)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-demo
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
    volumeMounts:
    - name: workdir
      mountPath: /usr/share/nginx/html
  # These containers are run during pod initialization
  initContainers:
  - name: install
    image: busybox:1.28
    command:
    - wget
    - "-O"
    - "/work-dir/index.html"
    - http://info.cern.ch
    volumeMounts:
    - name: workdir
      mountPath: "/work-dir"
  dnsPolicy: Default
  volumes:
  - name: workdir
    emptyDir: {}
```
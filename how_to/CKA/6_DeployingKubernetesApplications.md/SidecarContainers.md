# Using Sidecar Containers for Application Logging

[Sidecar](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/)

## Sidecar container
> container that enhances the primary application, for instance logging, monitoring, syncing

> A sidecar is a initContainer that has the restartPolicy field set to Always
> **Note**: The example from the documentation is from a deployment, if needed just a pod, ammend
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  labels:
    app: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: alpine:latest
          command: ['sh', '-c', 'while true; do echo "logging" >> /opt/logs.txt; sleep 1; done']
          volumeMounts:
            - name: data
              mountPath: /opt
      initContainers:
        - name: logshipper
          image: alpine:latest
          restartPolicy: Always
          command: ['sh', '-c', 'tail -F /opt/logs.txt']
          volumeMounts:
            - name: data
              mountPath: /opt
      volumes:
        - name: data
          emptyDir: {}
```
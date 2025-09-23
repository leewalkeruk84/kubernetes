# Pod Advanced

## Multi-container Pods

### Init container
> special container where the init container runs to completion before the main container is started 
> if init contianer fails, main container, will never start
[Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)
[Configure Pod Initialization](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-initialization/)

```bash
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

### Sidecar container
> container that enhances the primary application, for instance logging, monitoring, syncing
[Sidecar](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/)
> A sidecar is a initContainer that has the restartPolicy field set to Always
> **Note**: The example from the documentation is from a deployment, if needed just a pod, ammend
```bash
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

# Using port forwarding to access Pods
> Useful for testing Pod accessibity on a specific cluster node
> **Example**
> Create a single Pod to use as a test case
```bash
kubectl -n temp run -i -t nginx --image=nginx --restart=Never --port 80
```
> try to curl or wget - it will fail to connect
```bash
curl http://127.0.0.1:8080/
```
> Now turn on port forwading to that pod
```bash
kubectl -n temp port-forward nginx 8080:80
```
> now you should be able to access the pod locally from port 8080
```bash
curl http://127.0.0.1:8080/
```

# Jobs
> A job starts a Pod with the restartPolicy set to never
> spec.ttlSecondsAfterFinished to clean up completed Jobs automatically
> 3 different Job types, which is specified by completion and parallesim parameters
- Non-parallel Jobs. 1 pod started, unless Pod fails - completions=1 / parallelism=1
- Parallel jobs with fixed completion count. Job is complete after successfully running as many time as specified in jobs.spec.completions completions=n / parallelism=m
- Parallel jobs with a work queue, multiple jobs started, when one completes successfully, the job is complete. completions=1 / parallelism=n

> see examples
```bash
kubectl create job -h | less
```
> create the YAML file from here then can add new roperties before deployment
```bash
kubectl -n temp create job mynewjob --image=busybox --dry-run=client -o yaml -- sleep 5 > mynewjob.yaml
```
> output from above
```bash
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  name: mynewjob
  namespace: temp
spec:
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - command:
        - sleep
        - "5"
        image: busybox
        name: mynewjob
        resources: {}
      restartPolicy: Never
status: {}                            
```
> you can now add properties in the Job spec as needed
```bash
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  name: mynewjob
  namespace: temp
spec:
  completions: 4
  parallelism: 2
  ttlSecondsAfterFinished: 60
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - command:
        - sleep
        - "5"
        image: busybox
        name: mynewjob
        resources: {}
      restartPolicy: Never
status: {}                            
```
> apply hte config and check the output of jobs and pods
```bash
kubectl apply -f mynewjob.yaml
kubectl -n temp get jobs,pods
```

# CronJobs
```bash
kubectl -n temp create cronjob -h | less
```

```bash
kubectl -n temp create cronjob my-job --image=busybox --schedule="*/1 * * * *" -- date
kubectl -n temp create cronjob my-job --image=busybox --schedule="*/1 * * * *" --dry-run=client -o yaml > mynewcronjob.yaml -- date
```
> output from above
```bash
apiVersion: batch/v1
kind: CronJob
metadata:
  creationTimestamp: null
  name: my-job
  namespace: temp
spec:
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: my-job
    spec:
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
          - command:
            - date
            image: busybox
            name: my-job
            resources: {}
          restartPolicy: OnFailure
  schedule: '*/1 * * * *'
status: {}
```

## To test a CronJob, once a CronJob has been created, run
```bash
kubectl create job mytestjob --from=cronjob/mycronjob
kubectl -n temp get jobs,cronjobs.batch,pods
kubectl -n temp logs mytestjob-5mc5x 
.... Tue Sep 23 03:22:01 UTC 2025
```
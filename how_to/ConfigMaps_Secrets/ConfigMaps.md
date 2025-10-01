# ConfigMaps

## Providing Variables (manually) to Kubernetes Applications
>**note** manually rather than with ConfigMap or Secret

> Kubernetes doesnt offer a command line option to provide variables while running a deployment with kubectl create deploy
> You would have to use Kubernetes kubectl sev env deployment command after deployment creation

> **Example**
```bash
kubectl create deployment my-db --image=mariadb
kubectl get pods
kubectl describe pod my-db-7f4895957f-fvk9t
kubectl logs my-db-7f4895957f-fvk9t 
# [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified You need to specify one of MARIADB_ROOT_PASSWORD, MARIADB_ROOT_PASSWORD_HASH, MARIADB_ALLOW_EMPTY_ROOT_PA

# View the help file examples, and ammend example
kubectl set env --help | less
# Update deployment 'registry' with a new environment variable
# kubectl set env deployment/registry STORAGE_DIR=/local
kubectl set env deployment/my-db MARIADB_ROOT_PASSWORD=password
```

## Using Variables in Config Maps
> To create a ConfigMap use 'kubectl create cm' with either
> - --from-literal key=value
> - --from-env-file=/path/to/file
> - --from-file=/path/to/file

```bash
kubectl create cm -h | less
```

## Using Variables from Config Maps
> Easy way to use them is using 'kubectl set env'

```bash
kubectl set env --help | less

# Import environment from a secret
  kubectl set env --from=secret/mysecret deployment/myapp
  
  # Import environment from a config map with a prefix
  kubectl set env --from=configmap/myconfigmap --prefix=MYSQL_ deployment/myapp
  
  # Import specific keys from a config map
  kubectl set env --keys=my-example-key --from=configmap/myconfigmap deployment/myapp
```

> **Example**
```bash
kubectl create deploy mydb --image=mariadb --replicas=2

kubectl create cm -h | less
# kubectl create configmap my-config --from-literal=key1=config1
kubectl create cm mydbvars --from-literal=ROOT_PASSWORD=password

kubectl describe cm mydbvars

kubectl set env -h | less
#kubectl set env --from=configmap/myconfigmap --prefix=MYSQL_ deployment/myapp
kubectl set env --from=configmap/mydbvars --prefix=MARIADB_ deployment/mydb

kubectl get deployments.apps,pods
kubectl get deployments.apps mydb -o yaml
```

## Providing Configuration Files Using Config Maps
> In the data section of the ConfigMap, each file is referred to with its own key
> To use configuration files from Configmaps, the ConfigMap needs to be used as a Pod Volume and mounted on a directory 

> **Example**
```bash
echo "test file" > index.html

kubectl create cm -h | less
# kubectl create configmap my-config --from-file=key1=/path/to/bar/file1.txt
kubectl create cm myindex --from-file=index.html

kubectl describe cm myindex

kubectl create deploy myweb --image=mynginx
```
> add Volumes and volumeMount sections
> [Populate a Volume with data stored in a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#populate-a-volume-with-data-stored-in-a-configmap) 3/4 of way down page
```yaml
spec:
  containers:
  - image: mynginx
    imagePullPolicy: Always
    name: mynginx
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
  volumeMounts:
  - mountPath: /usr/share/nginx/html
    name: cmvol
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  terminationGracePeriodSeconds: 30
  volumes:
  - name: cmvol
    configMap:
      name: myindex
  
```
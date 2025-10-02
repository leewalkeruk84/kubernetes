# Understanding the API

> Main API documentation is here
> https://kubernetes.io/docs/reference/kubernetes-api/

```bash
# for info about resource types
kubectl api-resources
# for info about resource and version info
kubectl api-versions
```

## Connecting to the API
> To access the API using curl, start the kube-proxy on the kubernetes user workstation
```bash
kubectl proxy --port=8081 &
curl http://localhost:8081
```

## API Deprecations
>**Demo**
```bash
kubectl create -f redis-deploy.yaml
kubectl api-versions
kubectl explain --recursive deploy.spec | less
```
> Change the deprecated Kind from say beta to v1 or whatever is needed
> reapply the config and see what else complains. You may have to change some specs to match the new format in the docs

## Extending the API
> Creating Custom Resources using crds is a 2 part procedure
> - you need to define the resource, using the CustomResourceDefinition
> - after defining the resource, it can be added through its own API resource

> **Example**: Files in how_to/Working_With_the_API/CustomResourcesExample
```bash
cat crd-object.yaml
# create the resource from the manifest
kubectl create -f crd-object.yaml
# check backup resource is on Server
kubectl api-resources | grep backup

cat crd-backup.yaml
# create the backup
kubectl create -f crd-backup.yaml
kubectl get backups
```

```yaml
# crd-object.yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: backups.stable.example.com
spec:
  group: stable.example.com
  versions: 
  - name: v1
    served: true
    storage: true
    schema:
      openAPIV3Schema:
        type: object
        properties:
          spec:
            type: object
            properties:
              backupType:
                type: string
              image:
                type: string
              replicas:
                type: integer
  scope: Namespaced
  names:
    plural: backups
    singular: backup
    shortNames:
     - bks
    kind: BackUp
```

```yaml
# crd-backup.yaml 
apiVersion: "stable.example.com/v1"
kind: BackUp
metadata:
  name: mybackup
spec: 
  backupType: full
  image: linux-backup-image
  replicas: 5
```


> To find resources on the systemt that are created by CRDs, you have to look for the Kind field in the output of the kubectl api-resources command
```bash
kubectl api-resources | grep -i custom

 kubectl get crds
```
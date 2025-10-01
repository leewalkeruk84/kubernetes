# Secrets

> A secret is a base-64 encoded alternative for ConfigMaps
> 3 Types
> - generic: used for sensitive values like passwords
> - tls: stores tls keys
> - docker-registry: used to store registry access credentials

## Using Secrets in Applications
```bash
kubectl create secret -h | less
```

> depending on the type of secret, drill down further for examples
### generic
```bash
kubectl create secret generic -h | less
```

```bash
# Create a new secret named my-secret with keys for each file in folder bar
kubectl create secret generic my-secret --from-file=path/to/bar

# Create a new secret named my-secret with key1=supersecret and key2=topsecret
kubectl create secret generic my-secret --from-literal=key1=supersecret --from-literal=key2=topsecret

# Create a new secret named my-secret from env files
kubectl create secret generic my-secret --from-env-file=path/to/foo.env
```

### tls
```bash
kubectl create secret tls -h | less
```

```bash
# Create a new TLS secret named tls-secret with the given key pair
kubectl create secret tls tls-secret --cert=path/to/tls.crt --key=path/to/tls.key
```

### docker-registry
```bash
kubectl create secret docker-registry -h | less
```

```bash
# If you do not already have a .dockercfg file, create a dockercfg secret directly
kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
  
# Create a new secret named my-secret from ~/.docker/config.json
kubectl create secret docker-registry my-secret --from-file=path/to/.docker/config.json
```

## Using Secrets in Applications
> If it contains variables use kubectl set env
```bash
kubectl set env -h | less

# Import environment from a secret
kubectl set env --from=secret/mysecret deployment/myapp
```

> If it contains files, mount the secret
```bash

```

> **Example**
```bash
kubectl create secret generic -h | less
#kubectl create secret generic my-secret --from-literal=key1=supersecret
kubectl create secret generic dbpw --from-literal=ROOT_PASSWORD=password

kubectl get secret dbpw -o yaml
kubectl create deploy mynewdeploy --image=mariadb

kubectl set env --from=secret/dbpw deployment/mynewdeploy --prefix=MYSQL_
```


# Docker Registry Pull Private Image to Pod
[pull-image-private-registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/)

```bash
kubectl create secret docker-registry regcred --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: private-reg
spec:
  containers:
  - name: private-reg-container
    image: <your-private-image>
  # ref the docker-registry-secret you created earlier
  imagePullSecrets:
  - name: regcred
```
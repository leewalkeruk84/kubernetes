# Contexts

## Finding Contexts
### List contexts in `kubeconfig` file
```bash
kubectl config get-contexts
```

## Setting Contexts
### Set a context entry in `kubeconfig` file
Creates or updates a context with specified namespace, cluster, or user. Does not change the active context.
```bash
kubectl config set-context staging --namespace=staging
```
```bash
kubectl config set-context staging --namespace=staging --cluster=my-cluster --user=my-user
```

## Changing Contexts
### Switch to a context in `kubeconfig` file
Updates `current-context` in `kubeconfig`. Does not modify context settings (cluster, user, namespace).
```bash
kubectl config use-context dev
```
> **Important**: Ensure that you are using the correct context before running any kubectl commands
> ```bash
> kubectl config current-context
> ```
# Helm

> The main site for finding Helm repos is [ArtifactHub](https://artifacthub.io/)
> Search for software on there and run the commands to install it
> For example, to run the Kubernetes Dashboard

```bash
# Add kubernetes-dashboard repository
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

# Deploy a Helm Release named "kubernetes-dashboard" using the kubernetes-dashboard chart
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard

# or if you want to specify and create a namespace for it to un in
helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```

## Demo using Helm Repos
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami

helm repo list

helm search repo bitnami # search for the word 'bitnami'

helm search repo file # search for the word 'file'

helm search repo nginx --versions # shows the different versions
```

## Installing helm charts
> After adding repos, it is wise to ensure you have the most upto date info
```bash
helm repo update
```

> To install a chart with default parameters
> **note** a chart may be installed multiple times, so it is important to assign the right name to it
```bash
helm install <name> <chart>
```

> After installation, use helm list to list currently installed charts
```bash
helm list

helm list -n <namespace>
```

> To delete a chart
```bash
helm delete <chart-name>

helm delete <chart-name> -n <namespace>
```

## Demo Installing
```bash
helm install bitnami/mysql --generate-name

kubectl get all

helm show chart bitnami/mysql
helm show all bitnami/mysql
helm list
helm list --all-namespaces
helm status mysql-xxxx
```

## Managing Helm Applications
> Helm charts consists of templates to which specific values are applied
> Values are stored in the Values.yaml file, within the Heml chart
> Use helm show values to lsit current values (a lot)
> When installing the chart, you can use a custom values.yaml file, provided with --values values.yaml as a arg to the heml install

> Alternatively, use helm install .... --set key=value to set individual values

### Demo: Providing Custom Parameters
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update

# show the values file, and grep to find certain values that will be customized
helm show values bitnami/nginx
helm show values bitnami/nginx | grep commonLabels
helm show values bitnami/nginx | grep replicaCount

vim values.yaml

# added in the below values - without comments # obviously
# commonLabels: "type: helmapp"
# replicaCount: 3

helm install bitnami/nginx --generate-name --values values.yaml 

helm list
helm get values nginx-xxxx
helm get values --all nginx-xxxx
```

## Helm Upgrades 
> **Example: running a nginx installing without exposing it, then to upgrade it to make the application accessible
```bash
# use the --set to pass custom values
helm install bitnginx bitnami/nginx --set ingress.enabled=false
# use the --set to pass and override custom values 
helm install bitnginx bitnami/nginx --set ingress.enabled=true
```
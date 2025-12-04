# Creating a Template from a Helm Chart

You can create a set of yaml resources from a helm chart using the `helm template` command

# Example
```bash
# Add the Repo you need
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/

# Upadte the Repo
helm update

# Find the chart you need
helm search repo dashboard
# some of output from above                                
kubernetes-dashboard/kubernetes-dashboard	7.14.0       	           	

# Now create a template of the chart
helm template my-kube-dash kubernetes-dashboard/kubernetes-dashboard --version 7.14.0 > my-kube-dash-template.yaml
```

Now you have a set of resources that are all in YAML files that youn can deploy with `kubectl apply` instead of helm

## Example template with custom values

```bash
helm show values kubernetes-dashboard/kubernetes-dashboard > values.yaml

vim values.yaml
# change whatever values you need

# Now create a template of the chart
helm template my-kube-dash kubernetes-dashboard/kubernetes-dashboard -f values.yaml --version 7.14.0 > my-kube-dash-template-values.yaml

kubectl apply -f my-kube-dash-template-values.yaml

kubectl get all
```

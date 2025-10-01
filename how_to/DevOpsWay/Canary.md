# Canary
[Canary](https://kubernetes.io/docs/concepts/workloads/management/#canary-deployments)

## Ingress Based Canary
> New and Old application both have a SVC and a Ingress
> To know how to route to which Ingress, though the Annotations on each Ingress
> canary-weight annotation
Cant find in docs where the annotations for canary are listed - between mins 4 and 7 for where they are shown on vid for nginx controller
## Service Based Canary
> New and Old app would share a single SVC. Each deployment would match the selector on the SVC
> to load balance, say 25% to 75%, one deployment would have say 1 and the other 3
> If you ahve to create the deployments and add a extra label to each of them, when exposeing the application, you can use the --selector newlabel=value with kubectl expose deploy command
> To see if both deployments are being picked up, describe the service and see if you have 2 endpoints listed
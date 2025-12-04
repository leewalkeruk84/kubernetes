# Using Kustomize

`kustomize` is a Kubernetes feature that uses a file with the name `kustomization.yaml` to apply changes to a set of resources

This is convenient for applying changes to input files that the user does not control himself, and which contents may change because of new versions appearing in GIT

Use `kubectl apply -k ./` in the directory with the kustomization.yaml and the files it refers to to apply changes

Use `kubectl delete -k ./` in the same directory to delete all that was created by the Kustomization
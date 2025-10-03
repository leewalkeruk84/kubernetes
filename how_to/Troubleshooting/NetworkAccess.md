# Analyzing Network Access Problems

> Different elements are required when a user accesses an application
> - Through DNS, the request reaches the Ingress
> - Ingress has rules that connect the incoming requests to a Service by referring to the Service name
> - The Service connects to Pods based on labels and selectors

> To troubleshoot application access, analyze in the following order:
> - Does the DNS request reach the Ingress?
> ```bash curl myapp.local```
> ```bash ping myapp.local```
> - Is there an Ingress rule that connects the incoming request to a Service
> ```bash kubectl get ingress```
> ```bash kubectl describe myapp-ingress # look at backends output```
> - Does the Service connect to Pods addressing the right label
> ```bash kubectl get svc```
> ```bash kubectl describe myapp-svc```
> ```bash curl <ip-from-get svc command> # to test if you can get to the deployment from the service```
> ```bash kubectl get pod myapp-xxxx --show-labels```

> Checking Pod Ports
> - To test if a process is running as expected inside the Pod, but not accessible from the outside, use ``` bash kubectl port-forward```
> ```bash
> kubectl port-forward myweb 8080:80
> curl localhost:8080
> ```

## Netowrk Policy
> Use ```bash kubectl get netpol -n namepsace``` to list Networkpolicy for a specific namespace
> To check if the NetworkPolicy applies to a Pod, verify 'spec.podSelector' in the NetworkPolicy
> if a NetworkPolicy applikes to a Pod, it should have 'spec.ingress.podSelector.matchLabels' to define which Pods have access
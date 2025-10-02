# Observability

## Kubernetes API Health Endpoints
> Common practive, applications can be programmed to provide access to the /healthz endpoint to test application availablity

> The kube-apiserver itself exposes thress endpoints to test that it is working:
> - /healthz : returns 'ok' if API server is healthy> -
> - /livez : indicates the API server is alive
> - /readyz : indicates if the API server is ready to serve requests
> Use 'curl -k https://$(minikube ip):8443/healthz' to test
> **Note** Look carefully at result 'ok' maybe not start a new line, it maybe just before prompt

## Using Probes to Monitor Applications
> Probe itself is a simple test defined as a container property, which is often a command
> If the probe doesnt respond, the app is restarted

> Following probe tests are defined in pod.spec.container
> - exec : command is executed and returns a zero exit value
> - httpGet : HTTP request returns a response code betweem 200 and 399
> - tcpSocket : connectivity to a TCP socket (available port) is successful
> Can be configured with a 'failureThreshold' to determine how long it can take for app to react

> 3 different probe types
> - livenessProbe: check if app is alive. Container restarted if the probe fails
> - readinessProbe: check if app is ready tp serve requests. Container removed from list of available service if it fails
> - startupProbe: used to verify initial startup of the application. useful if startup can be slow. No other probes are used before this probe finishes successfully
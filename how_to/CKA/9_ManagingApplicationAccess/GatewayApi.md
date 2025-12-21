# Understanding Gateway API
Ingress is in feature freeze and no longer developed. The replacement is Gateway API

Gateway API adds more advanced features to manage incoming traffic:
- Advanced traffic management
- More options that are integrated in the API resources

Gateway API may find its way into future versions of the CKA exam

Do not run Gateway API on a node that already runs an Ingress controller

```bash
helm delete ingress-nginx -n ingress-nginx
```

## Gateway API Resources
Gateway API uses specific API resources which are provided as CRDs:
- `GatewayClass`: represents the Gateway Controller
- `Gateway`: defines an instance of traffic Handling infrastructure
- `HTTPRoute`: defines how traffic is routed to one or more Services

To work with Gateway API, a Gateway API Controller needs to be installed

Without this controller, theres nothing actually handling the incoming traffic!

### Gateway API Controller
Different Gateway API controllers are provided by the ecosystem

In this class, we'll use the Nginx Gateway Fabric, which is easily installed with helm

Before installing the controller, you must (currently) install the custom resources (CRDs)

# Configuring Gateway API
Gateway API uses specific API resources which are provided as CRDs:
- `GatewayClass`: represents the Gateway Controller
    - it uses `spec.controllerName` to connect to a specific Gateway controller
    - It has no further config, the real config is done on Gateway resource
- `Gateway`: defines an instance of traffic Handling infrastructure
    - Multiple Gateways can connect to one Gateway Controller
    - At least one Gateway is required
    - The Gateway uses the `gatewayClassName` property to connect to the `GatewayController`
    - It also defines `listeners` to specify which protocols should be serviced
- `HTTPRoute`: defines how traffic is routed to one or more Services
    - This defines which Service an incoming request should be forwarded
    - Incoming requests are identified by the `spec.hostnames`
    - The `parentRefs` propert connects the HTTPRoute to the Gateway
    - The `backendRefs` property connects to the HTTPRoute to a service

# Using Gateway API to Provide Access to Applications
## Labs
https://killercoda.com/chadmcrowell/course/cka/create-gateway-and-route

https://learning.oreilly.com/interactive-lab/cka-prep-using/9798341638266/
# Configuring Gateway API for TLS Access
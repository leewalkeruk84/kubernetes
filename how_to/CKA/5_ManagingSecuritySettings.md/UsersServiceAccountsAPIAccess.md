# Users, ServiceAccounts, and API Access

## Understanding Kubernetes Users
The Kubernetes API doesn't define users for people to authenticate and authorise

Users are obtained externally
- Defined by X.509 certificates
- Obtained from external OpenID-based authentication (Google, AD,...)
- ServiceAccounts are used to authorise Pods to get access to specific API resources
- Each Namespace has a ServiceAccount with the name default, which is used by Pods to get minimal access to Kubernetes resources
- Additional ServiceAccounts can be created if more access is needed
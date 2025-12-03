# Setting up RBAC for ServiceAccounts

## Configuring Roles
Roles are used on Namespaces and use Verbs to specify access to specific resources in that Namespace

Use `kubectl create role` to create roles

## Configuring RoleBindings
RoleBindings connect users or ServiceAccounts to Roles

Use `kubectl create rolebinding` to create it

## Creating ServiceAccounts
- A ServiceAccount is used to authorize Pods to get information from the API
- All Pods have a default ServiceAccount which provides minimal access
- If more access is needed, specific ServiceAccounts can be created
- ServiceAccounts don't have specific configuration, they are used in RoleBindings to get access to specfic Roles

## Demo
https://learning.oreilly.com/videos/certified-kubernetes-administrator/9780135375129/9780135375129-CKA4_01_05_05/
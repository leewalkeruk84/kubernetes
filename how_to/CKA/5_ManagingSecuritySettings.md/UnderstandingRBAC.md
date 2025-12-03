
## Understanding RBAC
> RBAC uses 3 components to grant permissions to API objects
> - Role consists of Verbs which assign specific permissions like view, edit and more
> - ServiceAccount is used by Pods that need access to API resources
> - RoleBinding connects a ServiceAccount or a User to a Role

>- A role binding grants the permissions defined in a role to a user or set of users. It holds a list of subjects (users, groups, or service accounts), and a reference to the role being granted. A RoleBinding grants permissions within a specific namespace whereas a ClusterRoleBinding grants that access cluster-wide.

> Role and RoleBindings have a Namespaced scope, ClusterRoles and ClusterRoleBindings have a cluster scope
> In RBAC users can be used for people that need access to specific resources
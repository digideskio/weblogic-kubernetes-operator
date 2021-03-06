> **WARNING** This documentation is for version 1.1 of the operator.  To view documenation for the current release, [please click here](/site).

# Role-Based Access Control (RBAC)

The operator assumes that certain roles and role bindings are created on the Kubernetes cluster.  The operator installation scripts create these, and the operator verifies that they are correct when the cluster starts up.  This document lists the [RBAC](https://kubernetes.io/docs/admin/authorization/rbac/) definitions that are created.

The general design goal is to provide the operator with the minimum amount of permissions that it requires, and to favor built-in roles over custom roles, where it make sense to do so.

## Kubernetes role definitions

| Cluster role | Resources | Verbs | Notes |
| --- | --- | --- | --- |
| `weblogic-operator-cluster-role` |	namespaces, persistentvolumes	| get, list, watch | 1 |
| |	customresourcedefinitions in API group apiextensions.k8s.io	| get, list, watch, create, update, patch, delete, deletecollection	| |
| |	domains in API group weblogic.oracle	| get, list, watch, update, patch	| |
| |	Ingresses in API group extensions	| get, list, watch, create, update, patch, delete, deletecollection	| |
| `weblogic-operator-cluster-role-nonresource`	| nonResourceURLs: ["/version/*"]	| get |	1 |
|`weblogic-operator-namespace-role`	| secrets, persistentvolumeclaims	| get, list, watch	| 2 |
| |	services, pods, networkpolicies	| get, list, watch, create, update, patch, delete, deletecollection | |
| `NAMESPACE-operator-rolebinding-discovery`	| system:discovery in API group rbac.authorization.k8s.io | |		1 |
| `NAMESPACE-operator-rolebinding-auth-delegator`	| system:auth-delegator in API group rbac.authorization.k8s.io	| |	1 |

**Notes**:

1. This cluster role is assigned to the operator’s service account in the operator’s namespace.  The uppercase text `NAMESPACE` in the cluster role name is replaced with the operator’s namespace.
2. This cluster role is assigned to the operator’s service account in each of the “target namespaces”; that is, each namespace that the operator is configured to manage.

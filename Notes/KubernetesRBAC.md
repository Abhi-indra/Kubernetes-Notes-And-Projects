# Role-Based Access Control (RBAC) in Kubernetes

Kubernetes RBAC is a critical aspect of securing your Kubernetes cluster, allowing you to define granular permissions for users and service accounts. While RBAC is conceptually simple, it's essential to implement it correctly to maintain a secure environment.

## Understanding RBAC Architecture:

Kubernetes RBAC is structured around three main components:

1. **Users/Service Accounts:**
   - Users are entities outside the Kubernetes cluster, while service accounts are internal entities used by applications running within the cluster.

2. **Roles/ClusterRoles:**
   - Roles define sets of permissions within a specific namespace, while ClusterRoles define permissions across the entire cluster.

3. **RoleBindings/ClusterRoleBindings:**
   - RoleBindings associate roles with users or service accounts within a specific namespace, while ClusterRoleBindings associate ClusterRoles with users or service accounts across the entire cluster.

## Creating Users in Kubernetes:

Kubernetes does not directly manage user accounts but delegates user management to identity providers. The API server, acting as an OAuth server, offloads user management to these providers. You can configure the API server with certain flags to enable this functionality, allowing users to authenticate using various identity providers such as LDAP or OIDC.

## Example: Role, ClusterRole, RoleBinding, and ClusterRoleBinding

### 1. Create a Role:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

### 2. Create a RoleBinding:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
- kind: User
  name: alice
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

In this example:
- We create a Role named "pod-reader" that allows the user to perform "get", "watch", and "list" operations on Pods within the "default" namespace.
- We create a RoleBinding named "read-pods" that binds the Role "pod-reader" to the user "alice" within the "default" namespace.

### 3. Create a ClusterRole:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: node-reader
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get", "watch", "list"]
```

### 4. Create a ClusterRoleBinding:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: read-nodes
subjects:
- kind: User
  name: bob
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: node-reader
  apiGroup: rbac.authorization.k8s.io
```

In this example:
- We create a ClusterRole named "node-reader" that allows the user to perform "get", "watch", and "list" operations on Nodes across the entire cluster.
- We create a ClusterRoleBinding named "read-nodes" that binds the ClusterRole "node-reader" to the user "bob" across the entire cluster.

# Difference between Role and ClusterRole in Kubernetes RBAC

In Kubernetes RBAC (Role-Based Access Control), both Role and ClusterRole are used to define sets of permissions, but they have different scopes and applications within the cluster. Understanding the difference between them is crucial for designing and implementing RBAC policies effectively.

## Role:

### Definition:
A Role is a Kubernetes resource that defines a set of permissions within a specific namespace. It allows you to specify what actions (verbs) a user or service account can perform on specific resources (API groups and resources) within that namespace.

### Scope:
Roles are scoped to a specific namespace. They are used to manage access control within that namespace only.

### Example Use Case:
You might create a Role named "pod-reader" in the "default" namespace that grants permissions to read Pods within that namespace.

## ClusterRole:

### Definition:
A ClusterRole is a Kubernetes resource that defines a set of permissions across the entire cluster. It allows you to specify what actions (verbs) a user or service account can perform on specific resources (API groups and resources) across all namespaces.

### Scope:
ClusterRoles are not bound to any specific namespace. They are used to manage access control across the entire cluster.

### Example Use Case:
You might create a ClusterRole named "node-reader" that grants permissions to read Nodes across all namespaces in the cluster.

## Difference Summary:

| Aspect           | Role                          | ClusterRole                   |
|------------------|-------------------------------|-------------------------------|
| **Scope**        | Specific Namespace            | Entire Cluster                |
| **Use Case**     | Manage Access within Namespace| Manage Access across Cluster |

**********************************************************************************
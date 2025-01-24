# Kubernates Network Addon

### Calico Network Addon Configuration

Calico is a networking and network security solution for Kubernetes clusters. The configuration includes:

- **DaemonSet**: Deploying Calico nodes across the cluster.
- **ServiceAccount**: RBAC for Calico components.
- **ClusterRoleBinding**: Bind the service account to necessary roles.

This setup helps with network policies and secure pod-to-pod communication.

For full configuration, refer to the [Calico](https://github.com/rspatel031/k8s-network-addons/blob/main/calico/).

---

### Flannel Network Addon Configuration

Flannel provides a simple network fabric for Kubernetes. The configuration contains:

- **DaemonSet**: Deploys Flannel pods across the nodes.
- **ServiceAccount**: Provides necessary RBAC permissions for Flannel.
- **ClusterRoleBinding**: Ensures Flannel has the required roles and permissions.

For full configuration, refer to the [Flannel](https://github.com/rspatel031/k8s-network-addons/blob/main/flannel/).

---

### Weave Network Addon Configuration

Weave provides a resilient network solution for Kubernetes clusters. The configuration includes:

- **DaemonSet**: Deploys Weave pods on all nodes in the cluster.
- **ServiceAccount**: RBAC for Weave networking.
- **ClusterRoleBinding**: Binds the necessary roles to the service account for Weave.

For full configuration, refer to the [Weave Net](https://github.com/rspatel031/k8s-network-addons/blob/main/weave/).

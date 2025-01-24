# Calico Network Add-on Installation Guide

This README provides comprehensive steps to install and configure the Calico network add-on in a Kubernetes cluster. The default Pod CIDR is `10.244.0.0/16`. If your cluster uses a different CIDR, update the configuration accordingly.

---

## Prerequisites

Before you proceed, ensure the following:

- A Kubernetes cluster is up and running.
- Administrative access to the cluster (`kubectl` is configured).

---

## Installation Instructions

Follow these steps to install the Calico network add-on:

### 1. Download the Calico Manifest

Retrieve the Calico manifest using the `wget` command:

```bash
wget https://raw.githubusercontent.com/rspatel031/k8s-network-addon/refs/heads/main/calico/calico.yaml -O calico.yaml
```

### 2. Review and Modify the Manifest

The default Pod CIDR in the manifest is set to `10.244.0.0/16`. If your cluster uses a different CIDR, open the `calico.yaml` file and update the configuration. Look for the following section in the manifest:

```yaml
- name: CALICO_IPV4POOL_CIDR
  value: "10.244.0.0/16"
```

Replace the value with your desired CIDR if necessary.

### 3. Apply the Manifest

Deploy Calico to your cluster by applying the manifest:

```bash
kubectl apply -f calico.yaml
```

### 4. Verify the Installation

Check the status of the Calico pods to ensure they are running correctly:

```bash
kubectl get pods -n kube-system
```

All Calico-related pods should display a `STATUS` of `Running`.

---

## Port Details for Calico

Ensure the following ports are open for proper communication between Calico components:

### Ports for Calico Components:
| Component          | Protocol | Port  | Description                                     |
|--------------------|----------|-------|-------------------------------------------------|
| BGP communication  | TCP      | 179   | Used for BGP peer communication (if enabled).  |
| Typha              | TCP      | 5473  | Used for Calico Typha to communicate with nodes.|
| Felix              | TCP/UDP  | 9091  | Metrics exposed by Felix.                      |
| Kubernetes API     | TCP      | 443   | Used by Calico components to communicate with Kubernetes. |

---

## Configuring Other Network Add-ons

If you wish to use alternative network add-ons, refer to the following configurations. These use the same Pod CIDR (`10.244.0.0/16`) by default. Update the CIDR if required.

### Calico:
```bash
wget https://raw.githubusercontent.com/rspatel031/k8s-network-addon/refs/heads/main/calico/calico.yaml -O calico.yaml
kubectl apply -f calico.yaml
```

### Flannel:
```bash
wget https://raw.githubusercontent.com/rspatel031/k8s-network-addon/refs/heads/main/flannel/flannel.yaml -O flannel.yaml
kubectl apply -f flannel.yaml
```

### Weave:
```bash
wget https://raw.githubusercontent.com/rspatel031/k8s-network-addon/refs/heads/main/weave/weave.yaml -O weave.yaml
kubectl apply -f weave.yaml
```

---

## Additional Notes

- **Network Policies:**  
  Calico supports Kubernetes-native network policies as well as its own advanced policies. These policies allow you to control traffic flow at the IP or port level, enhancing cluster security.

- **Default Pod CIDR:**  
  The default CIDR for Calico is set to `10.244.0.0/16`. Ensure you update the manifest to match your cluster's network configuration if needed.

---

By following the steps in this guide, you will successfully install and configure the Calico network add-on in your Kubernetes cluster. For additional details or troubleshooting, refer to the [Calico Documentation](https://docs.projectcalico.org/).

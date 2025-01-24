# Flannel Network Add-On for Kubernetes

This document provides detailed instructions for configuring the Flannel network add-on for Kubernetes using the `flannel.yaml` file from the repository.

## Default Configuration

- **Network Add-On**: Flannel
- **Default CIDR**: `10.244.0.0/16`

The `flannel.yaml` file can be downloaded using the following command:

```bash
wget https://raw.githubusercontent.com/rspatel031/k8s-network-addon/refs/heads/main/flannel/flannel.yaml
```

## Steps to Apply Flannel Network Add-On

1. **Download the YAML File:**

   Use the following command to download the Flannel configuration file:

   ```bash
   wget https://raw.githubusercontent.com/rspatel031/k8s-network-addon/refs/heads/main/flannel/flannel.yaml
   ```

2. **Apply the YAML File:**

   Run the following command to apply the Flannel configuration:

   ```bash
   kubectl apply -f flannel.yaml
   ```

3. **Verify the Setup:**

   Check if the Flannel pods are running successfully:

   ```bash
   kubectl get pods -n kube-system
   ```

   Ensure that the `kube-flannel-ds` pods are in a `Running` state.

---

## Ports Used by Flannel

Flannel uses the following ports for communication:

- **Port 8472/UDP**: Used for Flannel VXLAN data plane communication between nodes.
- **Port 4789/UDP** (if using VXLAN backend): VXLAN overlay traffic.
- **Port 6443/TCP**: Kubernetes API server communication (default port for Kubernetes API).
- **Custom Port Range (UDP)**: For inter-node communication when using Flannel with UDP backend.

Ensure these ports are open between all nodes in your Kubernetes cluster.

---

## Changing the Default CIDR

If you want to use a different Pod CIDR (other than `10.244.0.0/16`), follow these steps:

1. **Edit the YAML File:**

   Open the `flannel.yaml` file for editing:

   ```bash
   vim flannel.yaml
   ```

2. **Locate the `net-conf.json` Section:**

   Find the section where the `net-conf.json` is specified:

   ```yaml
   net-conf.json: |
     {
       "Network": "10.244.0.0/16",
       "Backend": {
         "Type": "vxlan"
       }
     }
   ```

3. **Update the `Network` Field:**

   Replace `10.244.0.0/16` with the desired CIDR, e.g., `192.168.0.0/16`:

   ```yaml
   net-conf.json: |
     {
       "Network": "192.168.0.0/16",
       "Backend": {
         "Type": "vxlan"
       }
     }
   ```

4. **Reapply the Configuration:**

   Save the file and reapply it:

   ```bash
   kubectl apply -f flannel.yaml
   ```

5. **Reinitialize the Cluster:**

   Ensure the new CIDR is reflected in your cluster initialization. For example, reinitialize the cluster with the new CIDR:

   ```bash
   kubeadm init --pod-network-cidr=192.168.0.0/16
   ```

---

## Troubleshooting

- **Pods Not Connecting**: Verify that the ports mentioned above are open between nodes.
- **Flannel Pods in CrashLoopBackOff**: Check the logs of the Flannel pods:

  ```bash
  kubectl logs -n kube-system <flannel-pod-name>
  ```

- **Incorrect CIDR**: Ensure that the CIDR specified in the `kubeadm init` command matches the CIDR in the `flannel.yaml` file.

---

## Additional Notes

- Flannel uses a VXLAN backend by default. If you wish to change the backend to `host-gw` or another option, modify the `Backend` field in `net-conf.json` accordingly.
- Always ensure that the CIDR used for Flannel does not overlap with any existing network in your infrastructure.

---

Feel free to modify the YAML file and configurations as per your requirements.

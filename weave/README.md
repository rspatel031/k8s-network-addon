# Weave Net Kubernetes Network Add-On

This guide explains how to set up the Weave Net add-on for Kubernetes networking using the file available at:

[Weave Net YAML File](https://raw.githubusercontent.com/rspatel031/k8s-network-addons/refs/heads/main/weave/weave.yaml)

## Default Configuration
- **Default CIDR**: `10.244.0.0/16`

Weave Net is a simple and powerful network add-on for Kubernetes that provides a container networking solution with minimal configuration. This guide also includes port details and steps to customize the configuration if required.

---

## Installation Steps

1. **Download the YAML File**:
   ```bash
   wget https://raw.githubusercontent.com/rspatel031/k8s-network-addons/refs/heads/main/weave/weave.yaml
   ```

2. **Apply the YAML File**:
   Deploy the Weave Net network add-on to your Kubernetes cluster:
   ```bash
   kubectl apply -f weave.yaml
   ```

3. **Verify the Deployment**:
   Ensure all the pods related to Weave Net are running:
   ```bash
   kubectl get pods -n kube-system
   ```

   Look for pods with names like `weave-net-*`.

---

## Port Details

Weave Net uses the following ports:

| **Protocol** | **Port** | **Description**                            |
|--------------|----------|--------------------------------------------|
| TCP          | 6783     | For communication between Weave peers      |
| UDP          | 6783     | For control messages between peers         |
| UDP          | 6784     | For data traffic between Weave peers       |
| TCP          | 6781     | For the Weave API (if enabled)             |

Ensure these ports are open between all nodes in your cluster for Weave Net to function properly.

---

## Customizing the CIDR

If you wish to use a different Pod Network CIDR instead of the default `10.244.0.0/16`, follow these steps:

1. **Edit the YAML File**:
   Open `weave.yaml` in a text editor:
   ```bash
   nano weave.yaml
   ```

2. **Locate the `env` Section**:
   Find the environment variable `WEAVE_NETWORK` in the YAML file:
   ```yaml
   - name: WEAVE_NETWORK
     value: 10.244.0.0/16
   ```

3. **Update the CIDR**:
   Replace `10.244.0.0/16` with your desired CIDR range.

4. **Apply the Changes**:
   Reapply the updated YAML file:
   ```bash
   kubectl apply -f weave.yaml
   ```

5. **Restart Pods**:
   Restart all Weave Net pods to reflect the changes:
   ```bash
   kubectl rollout restart daemonset weave-net -n kube-system
   ```

---

## Troubleshooting

### 1. **Weave Pods Not Starting**
   - **Error**: "Failed to allocate address"
     - **Solution**: Ensure there are no overlapping IP ranges with other networks. Update the CIDR in the YAML file as described above.

   - **Error**: "Network plugin not ready"
     - **Solution**: Check the `kubelet` logs on the affected node:
       ```bash
       journalctl -u kubelet
       ```

### 2. **Connectivity Issues Between Pods**
   - Ensure the required ports (`6783` and `6784`) are open between nodes.
   - Verify the Weave Net pods are running without errors:
     ```bash
     kubectl logs -n kube-system -l name=weave-net
     ```

### 3. **Weave IP Conflict**
   - If there are IP conflicts, try restarting the Weave pods:
     ```bash
     kubectl delete pod -n kube-system -l name=weave-net
     ```

### 4. **Firewall Issues**
   - Verify that your firewall settings allow communication on the required ports.

---

## Additional Resources

- Official Weave Net Documentation: [https://www.weave.works/docs/net/latest/kubernetes/kube-addon/](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/)

---

By following this guide, you can successfully deploy Weave Net as a networking solution for your Kubernetes cluster, customize the Pod Network CIDR, and troubleshoot common issues. Let me know if further assistance is needed!

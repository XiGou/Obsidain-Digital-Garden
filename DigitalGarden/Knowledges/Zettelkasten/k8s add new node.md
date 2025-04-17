### 添加 Worker 节点
1. **主节点**：

   ```bash
   kubeadm token create --print-join-command
   ```

   - 输出示例：`kubeadm join 192.168.1.100:6443 --token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:xxxxxx`

2. **新 Worker 节点**：

   ```bash
   sudo kubeadm join 192.168.1.100:6443 --token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:xxxxxx
   ```

3. **验证**：

   ```bash
   kubectl get nodes
   ```

---

### 添加 Control Plane 节点
1. **主节点**：

```bash
 kubeadm token create --print-join-command --certificate-key $(sudo kubeadm init phase upload-certs --upload-certs | tail -1)
```


2. **新 Control Plane 节点**：

   ```bash
   sudo kubeadm join 192.168.1.100:6443 --token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:xxxxxx --control-plane --certificate-key 1234567890abcdef...
   ```

3. **配置 kubectl**：

   ```bash
   mkdir -p $HOME/.kube
   sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```

4. **验证**：

   ```bash
   kubectl get nodes
   ```

---

### 前提
- 新节点已安装 `kubeadm`、`kubelet` 和容器运行时。
- 替换 IP、token 和 hash 为实际值。
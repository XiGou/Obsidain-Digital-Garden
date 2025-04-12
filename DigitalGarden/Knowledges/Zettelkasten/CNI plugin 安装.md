Kubernetes 报错 **`CNI plugin not initialized`** 通常表示 **容器网络接口 (CNI) 没有正确安装或初始化**，导致 Pod 处于 `ContainerCreating` 状态。可以按照以下步骤进行排查和修复：

---

## **1. 检查 CNI 相关 Pod 是否运行**

首先检查 CNI 相关的 DaemonSet（例如 `calico`, `flannel`, `cilium` 等）是否正常：

```sh
kubectl get pods -n kube-system
```

如果 CNI 相关的 Pod 处于 `CrashLoopBackOff` 或 `Pending`，则表示 CNI 没有正确启动。

---

## **2. 确保节点有 CNI 配置**

查看 `/etc/cni/net.d/` 是否有 CNI 配置文件：

```sh
ls -l /etc/cni/net.d/
```

正常情况下应有 CNI 配置文件，例如：

```sh
/etc/cni/net.d/10-calico.conflist
```

如果没有，请重新安装 CNI 插件。

---

## **3. 确保 CNI 二进制文件存在**

Kubelet 需要在 `/opt/cni/bin/` 目录找到 CNI 可执行文件：

```sh
ls -l /opt/cni/bin/
```

正常情况下应该包含：

- `bridge`
    
- `host-local`
    
- `loopback`
    
- `calico`
    
- `flannel`
    
- `cilium`
    
- `weave` 等

如果 `/opt/cni/bin/` 为空，则 CNI 没有正确安装，需要重新安装 CNI 插件。

---

## **4. 重新安装 CNI 插件**

如果你使用的是 `flannel`：

```sh
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

如果你使用的是 `calico`：

```sh
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

如果你使用的是 `cilium`：

```sh
kubectl apply -f https://raw.githubusercontent.com/cilium/cilium/v1.12/install/kubernetes/quick-install.yaml
```

---

## **5. 检查 `kubelet` 是否正常**

CNI 插件依赖 `kubelet`，如果 `kubelet` 没有正确指定 CNI 目录，则会失败：

```sh
journalctl -u kubelet -f | grep cni
```

如果看到如下报错：

```
failed to read CNI config /etc/cni/net.d: no such file or directory
```

则说明 CNI 目录没有配置正确，需要重启 `kubelet`：

```sh
systemctl restart kubelet
```

---

## **6. 重新创建 `kubeadm` 集群**

如果仍然无法解决，可以重置 Kubernetes 并重新初始化：

```sh
kubeadm reset -f
kubeadm init --pod-network-cidr=192.168.0.0/16
```

然后重新安装 CNI 插件。

---

## **总结**

|**步骤**|**检查内容**|**修复方法**|
|---|---|---|
|**1. 检查 CNI Pod**|`kubectl get pods -n kube-system`|重启 CNI Pod|
|**2. 检查 CNI 配置**|`ls -l /etc/cni/net.d/`|重新安装 CNI|
|**3. 检查 CNI 二进制**|`ls -l /opt/cni/bin/`|下载 CNI 可执行文件|
|**4. 重新安装 CNI**|`kubectl apply -f <CNI YAML>`|重新应用 CNI 配置|
|**5. 检查 kubelet**|`journalctl -u kubelet -f|grep cni`|
|**6. 重新初始化**|`kubeadm reset && kubeadm init`|重新安装 CNI|

🚀 **你用的是什么 CNI 插件？有没有 `kube-system` 里的 CNI Pod 日志？**
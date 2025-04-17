在 Kubernetes (`kubectl`) 中，可以使用以下命令来查看 Pod 内的容器列表、查看容器日志以及进入容器：

---

## **1. 查看 Pod 的容器列表**

```sh
kubectl get pod <pod-name> -o jsonpath="{.spec.containers[*].name}"
```

🔹 这个命令会输出该 Pod 内的所有容器名称。  
如果 Pod 有多个容器，也可以使用：

```sh
kubectl describe pod <pod-name>
```

🔹 在输出的 `Containers:` 部分，可以看到该 Pod 内的所有容器。

---

## **2. 查看 Pod 内某个容器的日志**

```sh
kubectl logs <pod-name> -c <container-name>
```

🔹 其中：

- `<pod-name>` 是 Pod 的名称。
    
- `<container-name>` 是 Pod 内的某个容器的名称（如果 Pod 只有一个容器，可以省略 `-c <container-name>`）。

如果需要**实时查看日志**（类似 `tail -f`），可以加上 `-f`：

```sh
kubectl logs -f <pod-name> -c <container-name>
```

如果要查看**以前的日志**（适用于容器崩溃重启的情况）：

```sh
kubectl logs --previous <pod-name> -c <container-name>
```

---

## **3. 进入 Pod 内的容器**

### **（1）进入 Bash 交互式终端**

如果容器内有 `bash`，可以使用：

```sh
kubectl exec -it <pod-name> -c <container-name> -- /bin/bash
```

🔹 `-it` 选项用于交互式会话。

### **（2）如果容器没有 `bash`，尝试使用 `sh`**

有些轻量级镜像（如 `alpine`）可能没有 `bash`，可以尝试：

```sh
kubectl exec -it <pod-name> -c <container-name> -- /bin/sh
```

### **（3）执行单个命令**

如果你只想在容器内运行一个命令（比如 `ls`），可以使用：

```sh
kubectl exec <pod-name> -c <container-name> -- ls /app
```

---

## **总结**

|任务|命令|
|---|---|
|查看 Pod 内的容器列表|`kubectl get pod <pod-name> -o jsonpath="{.spec.containers[*].name}"`|
|查看容器日志|`kubectl logs <pod-name> -c <container-name>`|
|实时查看日志|`kubectl logs -f <pod-name> -c <container-name>`|
|查看崩溃前日志|`kubectl logs --previous <pod-name> -c <container-name>`|
|进入 Bash 终端|`kubectl exec -it <pod-name> -c <container-name> -- /bin/bash`|
|进入 Sh 终端|`kubectl exec -it <pod-name> -c <container-name> -- /bin/sh`|
|在容器内执行命令|`kubectl exec <pod-name> -c <container-name> -- ls /app`|

如果你的 Pod 只有**一个容器**，`-c <container-name>` 可以省略。
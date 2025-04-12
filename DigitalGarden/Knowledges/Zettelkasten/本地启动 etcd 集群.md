以下是将三节点 etcd 集群的启动命令改为单行形式，并确保前台运行（不加 `&` 后台运行）。由于是单行命令，你需要在一个终端窗口运行一个节点，然后另开窗口运行其他节点，或者用脚本分步执行。这里提供每个节点的前台单行命令，方便复制粘贴：

---

### 单行命令（前台运行）

#### 节点 1（infra0）

```bash
etcd --name infra0 --data-dir etcd-cluster/infra0 --listen-client-urls http://127.0.0.1:2379 --advertise-client-urls http://127.0.0.1:2379 --listen-peer-urls http://127.0.0.1:2380 --initial-advertise-peer-urls http://127.0.0.1:2380 --initial-cluster infra0=http://127.0.0.1:2380,infra1=http://127.0.0.1:2381,infra2=http://127.0.0.1:2382 --initial-cluster-token etcd-cluster-1 --initial-cluster-state new --log-level info --logger zap
```

#### 节点 2（infra1）

```bash
etcd --name infra1 --data-dir etcd-cluster/infra1 --listen-client-urls http://127.0.0.1:2479 --advertise-client-urls http://127.0.0.1:2479 --listen-peer-urls http://127.0.0.1:2381 --initial-advertise-peer-urls http://127.0.0.1:2381 --initial-cluster infra0=http://127.0.0.1:2380,infra1=http://127.0.0.1:2381,infra2=http://127.0.0.1:2382 --initial-cluster-token etcd-cluster-1 --initial-cluster-state new --log-level info --logger zap
```

#### 节点 3（infra2）

```bash
etcd --name infra2 --data-dir etcd-cluster/infra2 --listen-client-urls http://127.0.0.1:2579 --advertise-client-urls http://127.0.0.1:2579 --listen-peer-urls http://127.0.0.1:2382 --initial-advertise-peer-urls http://127.0.0.1:2382 --initial-cluster infra0=http://127.0.0.1:2380,infra1=http://127.0.0.1:2381,infra2=http://127.0.0.1:2382 --initial-cluster-token etcd-cluster-1 --initial-cluster-state new --log-level info --logger zap
```

---

### 操作步骤
1. **创建数据目录**：

   ```bash
   mkdir -p etcd-cluster/{infra0,infra1,infra2}
   ```

2. **运行节点**：
   - 打开三个终端窗口。
   - 在每个终端分别粘贴并运行上述命令。
   - 每个命令会在前台运行，输出日志到终端。

3. **验证集群**：
   - 在第四个终端检查健康状态：

     ```bash
     ETCDCTL_API=3 etcdctl --endpoints=http://127.0.0.1:2379,http://127.0.0.1:2479,http://127.0.0.1:2579 endpoint health
     ```

---

### 注意事项
- **前台运行**：
  - 不加 `> logfile 2>&1 &`，日志直接输出到终端，按 `Ctrl+C` 停止节点。
- **单行粘贴**：
  - 每个命令已压缩为一行，方便复制。
- **顺序启动**：
  - 建议先启动 `infra0`，再启动其他节点，确保集群初始化正常。

---

### 如果需要单终端运行
由于前台运行无法在同一终端启动多个节点，你可以用 `tmux` 或 `screen` 分屏：
1. 安装 `tmux`：

   ```bash
   sudo apt install tmux
   ```

2. 启动 `tmux`：

   ```bash
   tmux
   ```

3. 分割窗口（每个窗口运行一个节点）：
   - `Ctrl+B` 然后 `"`（垂直分割）。
   - 在每个 pane 粘贴一个命令。

或者用脚本循环，但仍需手动开多个终端执行：

```bash
#!/bin/bash
for i in 0 1 2; do
  echo "Run in new terminal: etcd --name infra$i --data-dir etcd-cluster/infra$i --listen-client-urls http://127.0.0.1:2${i}79 --advertise-client-urls http://127.0.0.1:2${i}79 --listen-peer-urls http://127.0.0.1:238$i --initial-advertise-peer-urls http://127.0.0.1:238$i --initial-cluster infra0=http://127.0.0.1:2380,infra1=http://127.0.0.1:2381,infra2=http://127.0.0.1:2382 --initial-cluster-token etcd-cluster-1 --initial-cluster-state new --log-level info --logger zap"
done
```

---

### 验证
启动后运行：

```bash
curl http://127.0.0.1:2379/health
```

- 应返回 `{"health":"true"}`。


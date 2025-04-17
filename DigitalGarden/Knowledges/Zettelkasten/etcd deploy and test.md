[Slow etcd performance (performance testing and optimization) | Support | SUSE](https://www.suse.com/support/kb/doc/?id=000020100)

[本地启动 etcd 集群](本地启动%20etcd%20集群.md)

好的，根据你提供的三个 IP 地址（`192.168.30.189`、`192.168.24.192`、`192.168.28.4`），我将为你调整之前的 etcd 集群配置方案。这些 IP 地址看起来属于不同的子网，这可能会影响节点间的通信，因此需要确保网络配置允许这些节点互相访问（特别是端口 2379 和 2380）。以下是基于这三个 IP 的具体配置步骤。

---

### 前提条件
- 确保三台机器的网络互通，防火墙已开放 2379（客户端）和 2380（节点间通信）端口。
- 时间同步已配置（使用 `ntp` 或 `chrony`）。
- etcd 已安装（参考之前的安装步骤）。

---

### 步骤 1：配置 etcd 集群
以下是每个节点的配置文件，假设我们仍然使用 3 个节点，分别命名为 `etcd-1`、`etcd-2` 和 `etcd-3`。

#### 节点 1 配置（192.168.30.189）

```bash
sudo mkdir -p /var/lib/etcd

sudo mkdir -p /etc/etcd
echo '{
  "name": "etcd-1",
  "data-dir": "/var/lib/etcd",
  "listen-peer-urls": "http://192.168.30.189:2380",
  "listen-client-urls": "http://192.168.30.189:2379,http://127.0.0.1:2379",
  "initial-advertise-peer-urls": "http://192.168.30.189:2380",
  "advertise-client-urls": "http://192.168.30.189:2379",
  "initial-cluster": "etcd-1=http://192.168.30.189:2380,etcd-2=http://192.168.24.192:2380,etcd-3=http://192.168.28.4:2380",
  "initial-cluster-token": "etcd-cluster-1",
  "initial-cluster-state": "new"
}' | sudo tee /etc/etcd/etcd.conf
```

#### 节点 2 配置（192.168.24.192）

```bash
sudo mkdir -p /var/lib/etcd
sudo mkdir -p /etc/etcd
echo '{
  "name": "etcd-2",
  "data-dir": "/var/lib/etcd",
  "listen-peer-urls": "http://192.168.24.192:2380",
  "listen-client-urls": "http://192.168.24.192:2379,http://127.0.0.1:2379",
  "initial-advertise-peer-urls": "http://192.168.24.192:2380",
  "advertise-client-urls": "http://192.168.24.192:2379",
  "initial-cluster": "etcd-1=http://192.168.30.189:2380,etcd-2=http://192.168.24.192:2380,etcd-3=http://192.168.28.4:2380",
  "initial-cluster-token": "etcd-cluster-1",
  "initial-cluster-state": "new"
}' | sudo tee /etc/etcd/etcd.conf
```

#### 节点 3 配置（192.168.28.4）

```bash
sudo mkdir -p /var/lib/etcd
sudo mkdir -p /etc/etcd
echo '{
  "name": "etcd-3",
  "data-dir": "/var/lib/etcd",
  "listen-peer-urls": "http://192.168.28.4:2380",
  "listen-client-urls": "http://192.168.28.4:2379,http://127.0.0.1:2379",
  "initial-advertise-peer-urls": "http://192.168.28.4:2380",
  "advertise-client-urls": "http://192.168.28.4:2379",
  "initial-cluster": "etcd-1=http://192.168.30.189:2380,etcd-2=http://192.168.24.192:2380,etcd-3=http://192.168.28.4:2380",
  "initial-cluster-token": "etcd-cluster-1",
  "initial-cluster-state": "new"
}' | sudo tee /etc/etcd/etcd.conf
```

---

### 步骤 2：启动 etcd 服务
在每个节点上使用 systemd 启动 etcd：

1. 创建 systemd 单元文件（`/etc/systemd/system/etcd.service`）：

```bash
   cat <<EOF | sudo tee /etc/systemd/system/etcd.service
   [Unit]
   Description=etcd service
   After=network.target

   [Service]
   ExecStart=/usr/local/bin/etcd --config-file /etc/etcd/etcd.conf
   Restart=on-failure
   RestartSec=5

   [Install]
   WantedBy=multi-user.target
   EOF
```

2. 启用并启动服务：

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable etcd
   sudo systemctl start etcd
   ```

3. 检查状态：

   ```bash
   sudo systemctl status etcd
   ```

在所有三个节点上重复此步骤。

---

### 步骤 3：验证集群状态
在任意节点上使用 `etcdctl` 检查集群是否正常运行：

1. 设置环境变量：

   ```bash
   export ETCDCTL_API=3
   ```

2. 检查成员列表：

   ```bash
   etcdctl --endpoints=http://192.168.30.189:2379,http://192.168.24.192:2379,http://192.168.28.4:2379 member list
   ```

   输出应显示三个节点，例如：

   ```
   8e9e05c52164694d, started, etcd-1, http://192.168.30.189:2380, http://192.168.30.189:2379
   8e9e05c52164694e, started, etcd-2, http://192.168.24.192:2380, http://192.168.24.192:2379
   8e9e05c52164694f, started, etcd-3, http://192.168.28.4:2380, http://192.168.28.4:2379
   ```

3. 检查集群健康：

   ```bash
   etcdctl --endpoints=http://192.168.30.189:2379,http://192.168.24.192:2379,http://192.168.28.4:2379 endpoint health
   ```

   输出应为：

   ```
   http://192.168.30.189:2379 is healthy: successfully committed proposal: took = 1.234ms
   http://192.168.24.192:2379 is healthy: successfully committed proposal: took = 1.345ms
   http://192.168.28.4:2379 is healthy: successfully committed proposal: took = 1.456ms
   ```

---

### 步骤 4：测试读写
1. 写入数据：

   ```bash
   etcdctl --endpoints=http://192.168.30.189:2379 put /test-key "Hello, etcd cluster!"
   ```

2. 读取数据（从任意节点）：

   ```bash
   etcdctl --endpoints=http://192.168.24.192:2379 get /test-key
   ```

   输出应为：

   ```
   /test-key
   Hello, etcd cluster!
   ```

---

### 注意事项
1. **网络问题**：
   - 你的三个 IP 地址位于不同子网（`192.168.30.x`、`192.168.24.x`、`192.168.28.x`）。确保路由器或网络配置允许这些子网之间的通信。如果无法直接通信，可能需要配置网关或 VPN。
   - 测试连通性：

     ```bash
     ping 192.168.24.192  # 从 192.168.30.189 执行
     ping 192.168.28.4   # 从 192.168.30.189 执行
     ```

2. **防火墙**：
   - 检查并开放端口：

     ```bash
     sudo ufw allow 2379/tcp
     sudo ufw allow 2380/tcp
     ```

   - 或使用 `iptables`：

     ```bash
     sudo iptables -A INPUT -p tcp --dport 2379 -j ACCEPT
     sudo iptables -A INPUT -p tcp --dport 2380 -j ACCEPT
     ```

3. **生产环境**：
   - 启用 TLS 以加密通信。
   - 配置负载均衡器（如 HAProxy）提供统一的客户端访问地址。

如果启动时遇到错误（比如连接超时、节点未加入集群），请提供日志（`journalctl -u etcd`）或具体错误信息，我会帮你进一步排查！

## 监控

这里是一个简单的 Prometheus 配置文件 `prometheus.yml`，用于抓取 `etcd` 的指标（metrics）：

```yaml
global:
  scrape_interval: 15s  # 采集间隔

scrape_configs:
  - job_name: 'etcd'
    scheme: https
    tls_config:
      ca_file: /etc/kubernetes/pki/etcd/ca.crt
      cert_file: /etc/kubernetes/pki/etcd/healthcheck-client.crt
      key_file: /etc/kubernetes/pki/etcd/healthcheck-client.key
    static_configs:
      - targets:
		  - 'etcd-node1:2379'
		  - 'etcd-node2:2379'
		  - 'etcd-node3:2379'

```

[etcd pressure test](etcd%20pressure%20test.md)
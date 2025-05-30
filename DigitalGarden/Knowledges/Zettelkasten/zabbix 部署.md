> 最近需要调查 zabbix 相关问题，所以急需部署一个 zabbix，以下是 AI 生成内容，且经过本人在 ubuntu 24.04 实际测试通过。

下面是一个**完整、简洁的教程**，教你如何在 **Ubuntu 上使用 Docker 部署 Zabbix（含前端 + 数据库）**。

---

## ✅ 教程目标

在 Ubuntu 系统中，使用 Docker 快速部署一套 **Zabbix 6.x + PostgreSQL + Web UI** 的系统。

---

## 🧰 前置条件

- 已安装 Docker 和 Docker Compose
    
- 系统为 Ubuntu 18.04+，建议 20.04/22.04
    
- 至少 2GB 内存（推荐 4GB）

如果未安装 Docker：

```bash
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable docker --now
sudo usermod -aG docker $USER  # 使当前用户可以运行 docker（需要重启登录）
```

---

## 📦 步骤 1：准备 `docker-compose.yml`

创建一个目录：

```bash
mkdir ~/zabbix-docker && cd ~/zabbix-docker
```

创建 `docker-compose.yml`：

```yaml
version: '3.5'

services:
  postgres:
    image: postgres:15
    restart: unless-stopped
    environment:
      POSTGRES_USER: zabbix
      POSTGRES_PASSWORD: zabbix_pass
      POSTGRES_DB: zabbix
    volumes:
      - ./pgdata:/var/lib/postgresql/data

  zabbix-server:
    image: zabbix/zabbix-server-pgsql:alpine-6.0-latest
    restart: unless-stopped
    environment:
      DB_SERVER_HOST: postgres
      POSTGRES_USER: zabbix
      POSTGRES_PASSWORD: zabbix_pass
    ports:
      - "10051:10051" 
    depends_on:
      - postgres

  zabbix-web:
    image: zabbix/zabbix-web-nginx-pgsql:alpine-6.0-latest
    restart: unless-stopped
    environment:
      DB_SERVER_HOST: postgres
      POSTGRES_USER: zabbix
      POSTGRES_PASSWORD: zabbix_pass
      ZBX_SERVER_HOST: zabbix-server
      PHP_TZ: Asia/Shanghai
    ports:
      - "8080:8080"
    depends_on:
      - zabbix-server

  zabbix-agent:
    image: zabbix/zabbix-agent:alpine-6.0-latest
    restart: unless-stopped
    environment:
      ZBX_SERVER_HOST: zabbix-server
    depends_on:
      - zabbix-server
```

---

## 🚀 步骤 2：启动服务

在目录中执行：

```bash
docker-compose up -d
```

查看状态：

```bash
docker-compose ps
```

---

## 🌐 步骤 3：访问 Zabbix

打开浏览器，访问：

```
http://<你的Ubuntu主机IP>:8080
```

首次打开会进入初始化界面，点击下一步即可。默认账号密码：

-用户名：`Admin`  
-密码：`zabbix`

---

## 🛠️ 常见管理命令

- 停止服务：

    ```bash
    docker-compose down
    ```

- 查看日志：

    ```bash
    docker-compose logs -f
    ```

- 重启服务：

    ```bash
    docker-compose restart
    ```

---

## 📝 提示

- 你可以将 `POSTGRES_PASSWORD` 和 `PHP_TZ` 改成你需要的值。
    
- 如需挂载配置文件、自定义 Zabbix server.conf，可在容器中配置挂载。
    
- 若需长期部署，建议把 PostgreSQL 存储路径挂载至外部盘或独立容器卷。

---

## ✅ 完成！

你现在已经在 Ubuntu 上成功使用 Docker 部署了 Zabbix。如果你需要添加 Windows/Linux 主机的监控代理，或者集成邮件/钉钉告警，可以继续告诉我，我可以一步步帮你配置。

---

下面是 **在 Windows 上安装 Zabbix Agent 并连接到你 Docker 中部署的 Zabbix Server（在 Ubuntu 上）** 的完整步骤：

---

## ✅ 一、准备工作

你已经完成了：

- 在 Ubuntu 上部署了 Zabbix Server（通过 Docker Compose）
    
- 并知道了 Zabbix Server 的 **IP 地址或域名**

---

## ✅ 二、Windows 上安装 Zabbix Agent

### 1. 下载 Agent

前往 Zabbix 官网下载：

🔗 [https://www.zabbix.com/download_agents](https://www.zabbix.com/download_agents)

选择：

- Platform: `Windows`
    
- Version: `6.0 LTS`（或和你服务器一致的版本）
    
- Architecture: `amd64`
    
- Package type: `MSI`

> 示例文件名类似：`zabbix_agent-6.0.23-windows-amd64-openssl.msi`

---

### 2. 安装

双击 `.msi` 进行安装。安装向导中填写以下参数：

|项目|示例|
|---|---|
|Server or Proxy|`192.168.1.100`（Zabbix Server 的 IP）|
|Agent hostname|`windows-host-01`（可自定义）|

其他保持默认即可。

---

### 3. 安装后配置（可选）

配置文件路径：

```
C:\Program Files\Zabbix Agent\zabbix_agentd.conf
```

可编辑该文件手动调整：

```conf
Server=192.168.1.100         # Zabbix Server IP
ServerActive=192.168.1.100   # 同上
Hostname=windows-host-01     # 与 Zabbix 前端中的 Host 保持一致
```

修改后需重启服务：

```cmd
sc stop "Zabbix Agent"
sc start "Zabbix Agent"
```

---

## ✅ 三、在 Zabbix Web 前端添加主机

1. 登录 Zabbix Web UI
    
2. 进入 **Configuration → Hosts**
    
3. 点击右上角 **Create host**
    
4. 填写：

|字段|内容|
|---|---|
|Host name|windows-host-01|
|Group|`Windows servers`（可新建）|
|Agent interface|填 Windows IP，如 `192.168.1.101`|

5. 切换到 **Templates** 标签页，添加模板：

- `Template OS Windows by Zabbix agent`

6. 保存

---

## ✅ 四、验证

- 等待 1～2 分钟，在 **Monitoring → Latest Data** 页面中搜索主机名，应该能看到数据。
    
- 在 **Monitoring → Problems** 可以查看是否通信正常。

---

## 🛠️ 故障排查建议

|问题|排查方法|
|---|---|
|Agent 状态未上线|检查防火墙端口 10050（Agent 默认端口）是否开启|
|Server 无法主动发现|`ServerActive` 要设置为 Zabbix Server 的 IP|
|名称不一致|`Hostname` 与前端配置的 Host 名应严格一致|
|Agent 日志查看|`C:\Program Files\Zabbix Agent\zabbix_agentd.log`|

---

需要我也帮你写一份用于自动安装的 PowerShell 脚本吗？例如在企业场景中批量安装 Agent。
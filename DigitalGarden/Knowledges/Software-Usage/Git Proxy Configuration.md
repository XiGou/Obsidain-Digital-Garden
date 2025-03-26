### **Git 配置代理的方法**

Git 支持 **HTTP(S) 代理** 和 **SOCKS 代理**，可以针对全局或单个仓库进行配置。

---

## **1. 配置 HTTP(S) 代理**

适用于 `https://` 或 `http://` 方式的 Git 连接。

### **全局配置（影响所有 Git 仓库）**

```bash
git config --global http.proxy "http://代理IP:端口"
git config --global https.proxy "http://代理IP:端口"
```

**示例：**

```bash
git config --global http.proxy "http://127.0.0.1:7890"
git config --global https.proxy "http://127.0.0.1:7890"
```

### **单个仓库配置**

进入某个 Git 仓库目录，运行：

```bash
git config --local http.proxy "http://127.0.0.1:7890"
git config --local https.proxy "http://127.0.0.1:7890"
```

---

## **2. 配置 SOCKS 代理**

适用于 **Clash、V2Ray、Shadowsocks** 代理工具。

### **全局配置**

```bash
git config --global http.proxy "socks5://127.0.0.1:7891"
git config --global https.proxy "socks5://127.0.0.1:7891"
```

### **单个仓库配置**

```bash
git config --local http.proxy "socks5://127.0.0.1:7891"
git config --local https.proxy "socks5://127.0.0.1:7891"
```

---

## **3. SSH 协议代理**

Git 默认 **不支持** 直接通过 `git@github.com:xxx.git`（SSH 协议）配置代理，需要借助 `ProxyCommand` 或 `ProxyJump`。

### **方法 1：配置 `~/.ssh/config`**

编辑 `~/.ssh/config`，添加：

```ini
Host github.com
    ProxyCommand nc -X 5 -x 127.0.0.1:7891 %h %p
```

或者：

```ini
Host github.com
    ProxyCommand connect -S 127.0.0.1:7891 %h %p
```

如果是 `SOCKS5` 代理：

```ini
Host github.com
    ProxyCommand nc -X 5 -x 127.0.0.1:7891 %h %p
```

然后使用 `ssh` 方式 clone：

```bash
git clone git@github.com:xxx.git
```

### **方法 2：使用 `ProxyJump`（适用于 SSH 跳板机）**

```ini
Host github.com
    ProxyJump user@proxyserver
```

然后 `git clone` 走 SSH 代理。

---

## **4. 取消代理**

### **取消 HTTP 代理**

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### **取消 SOCKS 代理**

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

### **取消 SSH 代理**

编辑 `~/.ssh/config`，删除 `ProxyCommand` 配置。

---

## **5. 测试代理是否生效**

可以用 `git config --global --list` 查看代理配置：

```bash
git config --global --list | grep proxy
```

或者手动测试：

```bash
curl -x http://127.0.0.1:7890 https://github.com
```

如果 `curl` 能访问 GitHub，而 `git clone` 失败，说明可能是 `git` 代理未正确配置。

---

## **总结**

|代理类型|配置方式|
|---|---|
|**HTTP(S) 代理**|`git config --global http.proxy "http://127.0.0.1:7890"`|
|**SOCKS 代理**|`git config --global http.proxy "socks5://127.0.0.1:7891"`|
|**SSH 代理**|配置 `~/.ssh/config` `ProxyCommand`|
|**取消代理**|`git config --global --unset http.proxy`|

这样你就可以为 Git 配置代理，解决克隆慢、访问失败等问题 🚀。
---
tags:
  - Docker
---


# 从 docker.io 同步多架构镜像到内网 Harbor

在企业内网环境中，通常需要将 **Docker Hub 上的多架构镜像（multi-arch image）** 同步到内网私有仓库（如 Harbor），以便：

- 提升拉取速度
- 避免公网依赖
- 保证 CI/CD 稳定性
- 控制镜像版本

Docker 的多架构镜像实际上是 **manifest list + 多个架构镜像**，同步时需要确保 **manifest 也被同步**。

---

# 一、核心原理

多架构镜像结构：

```
image:tag
│
├── manifest list
│     ├── amd64 manifest
│     ├── arm64 manifest
│     └── arm/v7 manifest
│
├── layer blobs
```

如果只是 `docker pull` + `docker push`：

```
docker pull nginx:latest
docker push harbor/nginx:latest
```

默认 **只会同步当前架构**（例如 amd64）。

因此需要 **支持 manifest 的同步工具**。

常见方案：

|方法|推荐度|说明|
|---|---|---|
|skopeo|⭐⭐⭐⭐⭐|最推荐|
|docker buildx imagetools|⭐⭐⭐⭐|Docker官方|
|crane|⭐⭐⭐⭐|Google go-containerregistry|
|Harbor replication|⭐⭐⭐|Harbor自带|

---

# 二、方案1：使用 skopeo（最推荐）

Skopeo 是 RedHat 开源的容器镜像同步工具，**可以直接复制多架构镜像**。

## 安装

Linux：

```
apt install skopeo
```

或

```
yum install skopeo
```

---

## 登录 Harbor

```
skopeo login harbor.example.com
```

---

## 同步镜像

例如同步：

```
docker.io/library/nginx:1.27
```

到 Harbor：

```
harbor.local/library/nginx:1.27
```

命令：

```bash
skopeo copy --all \
  docker://docker.io/library/nginx:1.27 \
  docker://harbor.local/library/nginx:1.27
```

参数说明：

|参数|说明|
|---|---|
|`copy`|复制镜像|
|`--all`|**同步所有架构**|
|`docker://`|registry协议|

---

## 同步私有镜像

例如：

```
docker.io/bitnami/redis:7
```

同步：

```bash
skopeo copy --all \
  docker://docker.io/bitnami/redis:7 \
  docker://harbor.local/thirdparty/redis:7
```

---

# 三、方案2：使用 docker buildx

Docker Buildx 也可以复制 manifest。

## 创建 builder

```
docker buildx create --use
```

---

## 同步镜像

```
docker buildx imagetools create \
  -t harbor.local/library/nginx:1.27 \
  docker.io/library/nginx:1.27
```

该命令会：

1. 拉取 manifest
    
2. push 到 Harbor
    

优点：

- Docker原生支持
    

缺点：

- 对 registry 兼容性略差
    

---

# 四、方案3：使用 crane

crane 是 Google 开源工具。

安装：

```
go install github.com/google/go-containerregistry/cmd/crane@latest
```

同步：

```
crane copy \
  docker.io/library/nginx:1.27 \
  harbor.local/library/nginx:1.27
```

自动包含：

- manifest
    
- layer
    
- multi-arch
    

---

# 五、方案4：Harbor自带 Replication

Harbor 自带镜像同步功能。

配置流程：

1. Harbor UI
    
2. Administration
    
3. Registries
    
4. 添加 Docker Hub
    
5. Replication Rule
    

同步策略：

```
dockerhub -> harbor
```

优点：

- 自动同步
    
- UI管理
    

缺点：

- Docker Hub rate limit
    
- 灵活性较低
    

---

# 六、批量同步脚本示例

可以写一个批量同步脚本：

```bash
#!/bin/bash

REGISTRY=harbor.local/library

images=(
nginx:1.27
redis:7
postgres:16
)

for img in "${images[@]}"
do
  skopeo copy --all \
    docker://docker.io/library/$img \
    docker://$REGISTRY/$img
done
```

---

# 七、企业级最佳实践

推荐架构：

```
internet
    │
    │
mirror node
(skopeo sync)
    │
    │
Harbor registry
    │
    │
Kubernetes / CI
```

同步策略：

1. 定期同步基础镜像
    
2. Harbor作为唯一 registry
    
3. CI/CD 只访问 Harbor
    

---

# 八、常见问题

## 1 只同步了 amd64

原因：

没有 `--all`

正确：

```
skopeo copy --all
```

---

## 2 Harbor push 权限失败

需要：

```
skopeo login harbor.local
```

---

## 3 Docker Hub rate limit

建议：

- 使用登录 token
    
- 或者使用镜像代理
    

---

# 九、推荐工具总结

|工具|推荐指数|适合场景|
|---|---|---|
|skopeo|⭐⭐⭐⭐⭐|最通用|
|crane|⭐⭐⭐⭐|Go生态|
|buildx|⭐⭐⭐|Docker|
|Harbor replication|⭐⭐|自动同步|

**企业最推荐方案：**

```
skopeo copy --all
```

---

如果你需要，我可以再给你写一份 **“企业级镜像同步架构（自动化 + 缓存 + CI 加速）”**，很多公司（字节 / 阿里）都是类似架构，可以做到：

- 首次从 docker.io 拉
    
- 之后全内网秒级拉取
    
- 自动同步多架构镜像。
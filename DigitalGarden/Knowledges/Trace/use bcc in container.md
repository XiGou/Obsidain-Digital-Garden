# 在容器中使用 BCC

## 准备 Docker 环境

- 拉取包含 BCC 工具的 Docker 镜像：

```bash
 sudo docker run --rm -it --privileged -v /lib/modules:/lib/modules:ro -v /sys:/sys:ro -v /usr/src:/usr/src:ro ghcr.io/iovisor/bcc:ubuntu-20.04    
```

- `--privileged`：赋予容器特权，允许 BCC 工具访问内核。
- `-v /lib/modules:/lib/modules:ro`：挂载宿主机的内核模块。
- `-v /sys:/sys:ro`：挂载宿主机的 sys 文件系统。
- `-v /usr/src:/usr/src:ro`：挂载宿主机的内核源代码。

如果需要使用alpine环境，请执行下面的命令。

```bash
sudo docker run --rm -it --privileged -v /lib/modules:/lib/modules:ro -v /sys:/sys:ro -v /usr/src:/usr/src:ro --net=host registry.smtx.io/docker.io/alpine
```

- 更换 alpine 源为国内镜像源，加速下载。

```bash
sed -i 's#https\?://dl-cdn.alpinelinux.org/alpine#https://mirrors.tuna.tsinghua.edu.cn/alpine#g' /etc/apk/repositories

```

- 安装 BCC 工具：

```bash
apk add bcc-tools
```

## 使用 BCC 工具

- 现在，你可以在容器中使用 BCC 工具进行内核跟踪和性能分析。
- 例如，使用 `biolatency` 工具跟踪磁盘 I/O 延迟：

```bash
biolatency
```

- 其他 BCC 工具的使用方法请参考 BCC 文档。
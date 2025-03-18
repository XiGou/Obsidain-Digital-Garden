要将 CentOS ISO 挂载为本地软件包仓库（repo），可以按照以下步骤进行操作：

1. 创建一个目录来挂载 ISO 文件：

``` bash
sudo mkdir /mnt/iso
```

这将在 `/mnt/iso` 目录下创建一个用于挂载 ISO 的目录。

挂载 CentOS ISO 文件到指定目录：

```bash
sudo mount -o loop /path/to/centos.iso /mnt/iso
```

将 `/path/to/centos.iso` 替换为实际的 CentOS ISO 文件路径。这将将 ISO 文件挂载到 `/mnt/iso` 目录下。

2. 创建一个新的 repo 配置文件：

```bash
sudo vi /etc/yum.repos.d/centos-iso.repo
```

在编辑器中，添加以下内容：

```shell
[centos-iso]
name=CentOS ISO
baseurl=file:///mnt/iso
enabled=1
gpgcheck=0
```

保存并关闭文件。

1. 清除并重建 yum 缓存：

```bash
sudo yum clean all
sudo yum makecache
```

这将清除旧的 yum 缓存并重新构建缓存，使其包含新挂载的 ISO 文件中的软件包信息。

现在，你已经将 CentOS ISO 成功挂载为本地软件包仓库（repo）。你可以使用 yum 命令来安装、更新或删除软件包，它将从挂载的 ISO 文件中获取软件包。请确保 CentOS ISO 文件的路径和名称与上述步骤中提供的路径一致。
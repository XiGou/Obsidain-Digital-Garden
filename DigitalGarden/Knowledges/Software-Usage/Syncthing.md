---
date: 2024-01-06 01:14:31 +0800
dg-publish: true
title: Syncthing
---

开源的全平台支持的 P2P 同步工具

Golang 实现

Web UI 默认端口  `8384`

服务器部署需要修改 `ExecStart=/usr/bin/syncthing serve --no-browser  --gui-address=0.0.0.0:8384  --no-restart --logflags=0` 才能公网访问

If your PC has a local firewall, you will need to open the following ports for incoming and outgoing traffic:
- Port 22000/TCP: TCP based sync protocol traffic
- Port 22000/UDP: QUIC based sync protocol traffic
- Port 21027/UDP: for discovery broadcasts on IPv4 and multicasts on IPv6

## 【使用手册】之–自建 Syncthing 发现服务器和中继服务器

#系统 #Windows #教程 #工具 #手册 #Docker #Linux #Docker-compose

Syncthing 实现方法，首先通过发现服务器 (Discosrv)发现和索引用户，当用户设备接入发现服务器后，服务器会进行设备的连接通告，当设备之间同意连接后进行连接；两台设备建立连接后，数据需要通过 NAT 穿透的方式实现交换，如果 NAT 穿透不成功，就需要中继 (Relaysrv)服务器传输数据。Syncthing 官方的发现服务器 (Global Discovery)及社区贡献的中继服务器多集中在欧洲国内较少，使用 Syncthing 时经常会出现连接慢、时常断开、速度不稳定等问题。本篇利用一台国内的闲置服务器提供发现和中继服务加快同步效率，也可以解决隐私问题。

Syncthing 社区 Relay 服务器池位置： [https://relays.syncthing.net/](https://relays.syncthing.net/)

同步程序 syncthing： [https://github.com/syncthing/syncthing](https://github.com/syncthing/syncthing)

发现服务器 stdiscosrv： [https://github.com/syncthing/discosrv](https://github.com/syncthing/discosrv)

中继服务器 strelaysrv： [https://github.com/syncthing/relaysrv](https://github.com/syncthing/relaysrv)

文档： [https://docs.syncthing.net/index.html](https://docs.syncthing.net/index.html)

## 1\. Windwos Server 安装发现和中继服务

### 1.1. 发现服务

[https://github.com/syncthing/discosrv/releases](https://github.com/syncthing/discosrv/releases) 找到 windows 版本，下载解压后打开 cmd 运行：`stdiscosrv.exe -debug` 启动 stdiscosrv 服务，默认端口 8443 可以通过 `-listen ":8443"` 指定监听端口，其他参数参看 `-help`。程序启动后，记下生成的 `Server device ID`，后面添加发现服务时会用到。同时，在程序文件夹下自动生成同步发现服务器数据库 `discovery.db` 和服务器证书 `cert. pem、key.pem` 证书不变，重装后 `Server device ID` 不变。

### 1.2. 中继服务

[https://github.com/syncthing/relaysrv](https://github.com/syncthing/relaysrv) 找到 windows 版本，下载解压后打开 cmd 运行：`strelaysrv.exe -debug -pools="" -protocol=tcp4` 启动 strelaysrv 服务，默认端口 22067（连接端口）、22070（服务器状态端口），可以通过 `-listen ":22067"` 指定连接端口，`-pools=""` 不公开此服务器（默认会加入 Syncthing 官方 Relay 服务器池中共享中继服务器，会消耗大量流量，建议有流量上限的服务器不公开），`-protocol=tcp4` 只启用 TPv4 协议，其他参数参看 `-help`。程序启动后，记下生成的 `URI`，后面添加中继服务时会用到，同时，在程序文件夹下自动生成服务器证书。

**注意：服务器防火墙放行端口8443，22067。**

## 2\. Linux 使用 Docker 方式部署发现和中继服务

推荐使用 `docker-compose` 方式部署，`yaml` 文件如下：

获取发现服务的 `Server device ID` 和中继服务的 `URI`

**注意：服务器防火墙放行端口8443，22067。**

## 3\. Syncthing 中配置发现和中继服务器

打开 Syncthing `操作` -> `设置` -> `连接`

![](https://image.vlinyu.com/i/2023/04/01/syncthingdr01.png)

如果希望保留 Syncthing 官方发现服务器和社区中继服务器，可以在地址前加上 `default,` 如：
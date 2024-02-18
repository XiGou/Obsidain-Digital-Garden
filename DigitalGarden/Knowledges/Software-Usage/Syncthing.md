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
---
title: Alpine Linux Usage
---

## Install

```bash
setup-alpine
```

when setting apk mirror, don't use auto select fastest mirror, it takes a long time.

[Installation - Alpine Linux](https://wiki.alpinelinux.org/wiki/Installation)

## Install Desktop & xrdp

[Xfce - Alpine Linux](https://wiki.alpinelinux.org/wiki/Xfce)  
[Remote Desktop Server - Alpine Linux](https://wiki.alpinelinux.org/wiki/Remote_Desktop_Server)

```bash
setup-desktop

alpine:~# rc-service lightdm start
alpine:~# rc-update add lightdm
```

```bash
apk add xrdp xorgxrdp
rc-service xrdp start 
rc-service xrdp-sesman start
rc-update add xrdp
rc-update add xrdp-sesman
```
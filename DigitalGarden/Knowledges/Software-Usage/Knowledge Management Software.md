---
date: 2024-01-06 01:14:31 +0800
title: "Knowledge Management Software"
---

# Appication

## Joplin

[Joplin](https://joplinapp.org/)         
Open source, But the UI design is terrible, I don't like it.

---
# self-host
## Trilium
UI 简单够用，审美风格 Geek
功能基本都有，甚至支持一些奇技淫巧，通过在笔记内嵌 JS 代码实现特殊功能，不过这是典型的技术宅思维，不看好这种私有的另辟蹊径的方式，就算是做这样的事情，也应该做成开放的插件系统，而不是将笔记和实现功能的代码混在一起。
支持 Docker 镜像集中部署服务，支持本地客户端，同时支持多个服务实例和客户端之间配置 P2P 同步。

## Affine Notes
等待 Self Host Release

## SilverBullet

[Client Modes](https://silverbullet.md/Client%20Modes)  
使用体验非常极客，非常简洁

## Logseq
[logseq/docs/docker-web-app-guide.md at master · logseq/logseq · GitHub](https://github.com/logseq/logseq/blob/master/docs/docker-web-app-guide.md)

```
docker pull ghcr.io/logseq/logseq-webapp:latest
docker run -d --rm -p 0.0.0.0:3001:80 ghcr.io/logseq/logseq-webapp:latest
```

But actually, you cannot store your data on the server, the self-host version is just a web-based desktop App,  I think it is useless.

---
dg-publish: false
date: 2024-01-01 00:22:33
description: 苟熙 个人博客 gouxi xigou 程序员 Developer
---

## Jul 4, 2025

Openstack 太坑爹了，部署了一个测试集群，发现所有 vm 都卡在 scheduling，各种操作都试过了，没有什么思路。

现象是 nova 服务在对 vm 进行 schedule 之后就卡住，尝试如下方向：
- 查看 nova-conductor 服务日志，没有报错
- nova-compute 节点也没有收到任何信息，打开 debug 日志也没看到任何 vm 调度过来的痕迹
- nova-scheduler 日志内能看到调度器正常工作并选定一个节点
- glance 服务没有任何异常迹象
- 打开 rabbitmq 的 trace 日志，没有什么新的内容，似乎无法看到消息队列内的每一条消息
- 找到了一篇很高质量的博客，[[../../Collections/Clippings/OPENSTACK 新建实例无限SCHEDULING]], 详细讲述了一种可能导致永远 scheduling 的机制，但是经过仔细检查，并非是此问题。  
最后的最后，我发现自己傻逼至极，在配置的如下部分禁用了 conductor，导致没法实现透过 conductor 向 nova-compute 通信，改成 false 就好了....

```bash
[conductor]

#
# Options defined in nova.conductor.api
#

# Perform nova-conductor operations locally (boolean value)
use_local=true
```

当然此处如果 controller 和 compute 节点同时使用 true 估计也是能用，但是我懒得再去验证了，太恶心了。
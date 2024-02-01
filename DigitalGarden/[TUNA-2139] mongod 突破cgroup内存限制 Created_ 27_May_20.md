---
local.2 00007f2d590bc000 2096128 1738436 0 rw-s- local.2 00007f2dd8fbc000 2096128 32 32 rw
---
 local.1
00007f2e58ebc000 2096128 1086472 584 rw-s- local.1
00007f2ed8dbc000 65536 4 4 rw--- local.0
00007f2edcdbc000 65536 2088 4 rw-s- local.0
---------------- ------- ------- -------
total kB 14149084 3014936 62652

# 17.50
Address Kbytes RSS Dirty Mode Mapping
00007f0b6ed00000 2096128 4 0 rw--- local.2
00007f0beec00000 2096128 1434608 0 rw-s- local.2
00007f0c6eb00000 2096128 0 0 rw--- local.1
00007f0ceea00000 2096128 1608744 116 rw-s- local.1
00007f0d6e900000 65536 0 0 rw--- local.0
00007f0d72900000 65536 2092 4 rw-s- local.0
---------------- ------- ------- -------
total kB 14134668 3248168 60516

# 17.51
Address Kbytes RSS Dirty Mode Mapping
00007f922a648000 2096128 4 0 rw--- local.2
00007f92aa548000 2096128 1648112 0 rw-s- local.2
00007f932a448000 2096128 12 12 rw--- local.1
00007f93aa348000 2096128 15800 288 rw-s- local.1
00007f942a248000 65536 4 4 rw--- local.0
00007f942e248000 65536 2092 8 rw-s- local.0
---------------- ------- ------- -------
total kB 14169704 1852624 61356

# 17.52
Address Kbytes RSS Dirty Mode Mapping
00007fc1a43a4000 2096128 4 0 rw--- local.2
00007fc2242a4000 2096128 1547912 0 rw-s- local.2
00007fc2a41a4000 2096128 20 20 rw--- local.1
00007fc3240a4000 2096128 46688 496 rw-s- local.1
00007fc3a3fa4000 65536 12 4 rw--- local.0
00007fc3a7fa4000 65536 2092 4 rw-s- local.0
---------------- ------- ------- -------
total kB 14472844 1899152 125936

# 17.53
Address Kbytes RSS Dirty Mode Mapping
00007f969fecf000 2096128 36 36 rw--- local.1
00007f971fdcf000 2096128 1048580 720 rw-s- local.1
00007f979fccf000 65536 4 0 rw--- local.0
00007f97a3ccf000 65536 0 0 rw-s- local.0
---------------- ------- ------- -------
total kB 22402580 1442376 65436

通过 pmap 对 5 个主节点 mongod 进程内存占用情况分析结果来看, 占用内存较大的主要是映射到内存中的 local db , local db 主要用来存储 oplog. 由于 17.49/51 节点内存限制失效, 导致 local db 映射进内存量逐渐增大超过 2GB.

集群升级时间点及对应 MongoDB 内存状态统计
2021-02-04: 升级操作, 此次升级操作后, 各节点均未出现 MongoDB 内存限制失效问题;
2021-03-17: 升级操作, 此次升级操作后, 17.49/50 两节点 MongoDB 内存限制失效, 升级完成一段时间后内存增长超过 2GB;
2021-03-22: 升级操作, 此次升级操作后, 17.49/50 两节点 MongoDB 内存限制失效问题依然存在, 仍然在升级一段时间后增长超过 2GB;
对应 prometheus 监控数据如截图所示:
检查各节点 mongod cgroup 信息异常点如下:
17.49/51 节点在 03-17 升级阶段, MongoDB 重启时, cgroup 相关文件被重建, 其余节点 cgroup 相关文件仍然为 02-04 时间点;
Comment by Quan Zou [ 02/Aug/21 ]
https://github.com/krareT/trkdb/wiki/Notes-about-mmap-and-cgroup

Comment by Tianxin Yang [ 21/Apr/22 ]
product: SMTXOS
version: 4.0.12
rc_version: 4.0.12
build_date: 2022-04-16 14:15:37

 

4.15 部署

4.18 16:38 - 17:05 升级

4.20 10:00 左右开始超过限制，逐渐增长



 

CGroup 显示未超过限制

[root@node-19-202 11:34:48 smartx]$systemctl status system-mongod.slice
● system-mongod.slice - mongod Slice
Loaded: loaded (/etc/systemd/system/system-mongod.slice; static; vendor preset: disabled)
Drop-In: /etc/systemd/system/system-mongod.slice.d
└─50-MemoryLimit.conf
Active: active since Fri 2022-04-15 15:48:47 CST; 5 days ago
Tasks: 240
Memory: 1.9G (limit: 2.0G)
CGroup: /system.slice/system-mongod.slice
└─mongod.service
└─101064 /usr/bin/mongod --quiet -f /etc/mongod.conf run

 

cat /sys/fs/cgroup/memory/system.slice/system-mongod.slice/cgroup.procs
101064

 

进程显示内存使用已经超过上限2G



 

pmem 查看内存使用

00007f18e9223000 2096128 4 0 rw--- local.2
00007f1969123000 2096128 2096120 0 rw-s- local.2
00007f19e9023000 2096128 20 20 rw--- local.1
00007f1a68f23000 2096128 1415008 0 rw-s- local.1
00007f1ae8e23000 65536 0 0 rw--- local.0

 

 

Comment by Tianxin Yang [ 13/Jan/23 ]
类似问题复现

https://smartx1.slack.com/archives/C04AR86S7TL/p1673319765132279?thread_ts=1673316809.541629&cid=C04AR86S7TL

 

一种复现方式：

创建memory cgroup，服务运行在该memory cgroup中。服务使用的文件被被cache到内存中，并统计到该memory cgroup中。
停止服务，删除memory cgroup。 此时服务使用的文件仍然cache在内存中
重新创建memory cgroup，启动服务，服务复用了cache中文件页，但是这些cache的页没有统计到重建的memory cgroup中。
Comment by Tianxin Yang [ 13/Jan/23 ]
解决方法可参考

https://engineering.linkedin.com/blog/2022/overcoming-challenges-with-linux-cgroups-memory-accounting 

Generated at Tue Aug 15 05:57:34 UTC 2023 by Xi Gou using Jira 8.13.17#813017-sha1:0d8e32d02b922ca79b10f4de35650a105a1a6ad1.
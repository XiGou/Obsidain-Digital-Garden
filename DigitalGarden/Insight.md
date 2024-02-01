一共需要解决三个问题：
1.  slow
2. 故障定位
3. 故障预警

具体的事情：
首先总结所有慢的case （先collect 10个review）


| sdf | sf |
|---|---|
| sdf | sf |


**

1. 应当关注 Cache 命中率低，磁盘本身的负载高
    
2. 应当关注接入点分布不均匀
    
3. 应当关注节点级别、磁盘级别的 IO 负载不均
    
4. 应当关注任意两节点之间的写入延迟过高
    
5. 应当关注 HCI 模式 Volume 的副本是否本地化
    
6. 应当关注集群内部的网络状态
    
7. 应该有一个在生产集群快速测试存储带宽，延迟的标准流程，应该包含 IO 各个阶段延迟数据
    
8. 应当关注 IO Reroute 工作是否正常 （指向）
    
9. 应当关注 IO Merge 是否开启，工作状况
    
10. 应当关注 IO Pattern 是否对 ZBS 友好
    
11. 应当关注单节点的配置，如缓存比例
    
12. 应当关注迁移情况，如：速率、是否均衡
    
13. 应当关注 CGroup 配置是否正确且生效
    

**


在目前 ZBS 团队的所有售后 Case 中，“IO 慢” 是一个比较常见的问题，这类问题现象种类多，根本原因也较为复杂，因为我们目前只能从文档了解 ZBS 的软件架构和 IO 链路，然后通过 Linux 系统工具和运维命令逐一排查，由于导致问题的可能性较多，排查工作量大，同时问题能否调查清楚并顺利解决也强依赖于工程师的个人经验。
通过对 ZBS 所有关于 IO 慢的售后 Case 的总结，此类问题大致可以分为以下类型：
1. IO 实际并不慢，用户使用的测试流程有误；
2. ZBS 集群内部的问题，例如网络延迟丢包；
3. ZBS 软件本身的问题，例如副本没有本地化分配、迁移卡住、部分模块未开启或者工作不正常；
4. ZBS 软件可感知但无法处理的问题，例如缓存命中率低、IO 负载不均衡；
5. ZBS 系统之外的问题，如 ELF 和 Vmware 部署形态下，虚拟化平台的配置问题；
因此为了使得 IO 慢的问题能够简单快速地完成定位，目前计划基于可观测性平台, 通过 ZBS Insight 服务完成相应的功能。

# IO 慢的判定
有一些 Case 是由于用户自行测试 ZBS 存储系统的速度，但是测试的方法或参数不正确，导致测试结果看起来很慢，但实际上 ZBS 系统运行完全正常；另一种情况就是用户感觉业务变慢了，而这个信息比较模糊，我们需要尝试复现并观察才能更明确地认识到是什么样的场景下发生 IO 慢的情况，这个 IO 慢的现象程度如何。
上述两种情形主要的原因是我们没有一个标准化的在生产环境中探测存储速度的路径，要实现这样的一个功能有如下几种路径：
## 从 ZBS 存储切入
1. 建立测试 Volume 进行读写测试
2. 对于有生产数据的 Volume 进行读测试
这个测试的流程可以有如下实现的可能方式
- 集成 fio 等测试工具完成，这样的测试好处是符合真实场景，缺点是触发测试以后会占用性能
- 通过探测的方式完成，在 ZBS IO 路径上的 Trace 功能打开的情况下，发送探测包，然后通过 Trace 数据查看 IO 各个阶段的耗时
## HCI + Elf 场景侵入 VM 内部
性能问题不光可能出现在 ZBS 系统内部，也有可能出现在 VM 到存储之间，通过侵入 VM 内部获取的数据可以帮助判断 IO 慢是否是由于 ZBS 系统内部的问题导致。
目前我们有 SMTX VM Tools（SVT），它可支持以下功能：
- 实时收集 VM Guest OS 的配置信息
- 实时收集 VM Guest OS 的性能信息
- 通过 ELF 直接修改 VM 的配置
可以通过 SVT 搜集内部性能数据，辅助判断；更进一步，可以扩展这个工具，在 VM 内部进行测试获得测试数据。

## 通过分析 Trace 数据实现 （TODO）
目前可观测性平台 trace 尚未实现

# 服务架构的可视化
目前我们的服务结构主要依靠技术文档展示，对于生产环境，用户很难认知到服务架构，也无法获知各个组件的健康状况和一些关键的数据。
可以构建一个视图，用于展示集群内的服务拓扑和运行状态。
只对于 Meta， Chunk， ISCSI-Redirector 这样的主要服务进行展示，主要展示其关键属性、资源占用、各种任务/队列的数量等能反映其负载情况的数据。
例如展示每个节点的 Chunk 接入数量， 就可以辅助 [case 1052062](https://cs.smartx.com/cases/detail?id=1052062) 发现接入点分布不均的问题；
展示每个 Chunk 的 IO 压力数据，就可以帮助发现 IO 负载不均衡的问题。
# IO 链路可视化
将 IO 链路中的各个阶段的耗时以 Span 图形式展示出来，能够非常清晰地判断耗时导致 IO 耗时较多的具体阶段，但是这个功能的实现依赖于 Trace， Trace 目前对于 ZBS 的性能影响较大，且尚未在可观测性平台实现。

> Span 图示例
## Slow IO 日志
以下是一个 ZBS Slow IO 的日志，目前可以通过解析 Slow IO 日志可视化展示某一个具体的 Slow IO 的部分链路，部分完成这个功能。
```bash
W1026 11:15:51.294374 37094 access_io_handler.cc:265] [ACCESS SLOW IO TRACE] volume_id: "a5d3261d-c1e5-4756-8dbf-ec85d0f928d6", vextent_no: 2, pid: 6985, epoch: 9077, data_len: 262144, extent_offset: 241172480, flags: 0 , elapsed_ms: 11700, expect_event_num: 7
{ ACCESS_IO_BEGIN: rel_time_us=0 }
{ ACCESS_WAIT_RECOVER: rel_time_us=0 }
{ ACCESS_REPLICA_IO_BEGIN: rel_time_us=0 cid=3 data_len=262144 extent_offset=241172480 flags=0 gen=918479 op=3 pid=6985 }
{ ACCESS_REPLICA_IO_BEGIN: rel_time_us=0 cid=1 data_len=262144 extent_offset=241172480 flags=0 gen=918479 op=3 pid=6985 }
{ ACCESS_REPLICA_IO_END: rel_time_us=24000 cid=1 error_code=0 op=3 pid=6985 }
{ ACCESS_REPLICA_IO_END: rel_time_us=11688952 cid=3 error_code=2038 op=3 pid=6985 }
{ ACCESS_IO_END: rel_time_us=11700952 error_code=2034 }
```
## Trace 数据（TODO）
若有永远开启的 Trace 数据，或者可以通过开关控制其在探测期间完全开启的 Trace 数据，该功能将会极大增强。

# 网络负载情况
目前smartx已有网络流量可视化产品，可以分析宿主机之间的网络负载情况，可以通过这些数据判定 ZBS 节点之间的存储网络和管理网络运行状况，主要考虑以下问题：
- 是否存在存储网络管理网络混用
- 是否存在丢包、高延迟现象
目前实际上已经有节点级别的 metric 可以实现此能力，目前需要做的是在默认的监控内增加默认的视图来发现此类问题，同时在故障定位的模块中自动分析此项。
## 更细粒度
在 ZBS 内部，IO Payload 应该是占据绝大部分的网络带宽的，在分析时往往可以不考虑其他部分的网络消耗。但是如果能进一步细化到各个服务/功能（例如对 Chunk 内的 IO 和 Recover 进行区分）的网络性能数据，则能够使得数据更加精确可靠。
# ZBS 内部状态暴露
## IO Pattern
不同的上层业务对于存储的使用方式有很大的差别，对于 ZBS 来讲则表现为不同的 IO Pattern，不同的 IO Pattern 对于 ZBS 来讲性能也有较大差距，增加一个热力图展示 Volume IO 的热力图可以帮助理解实时的 IO Pattern 信息。
参考文章[我眼中的分布式系统可观测性](https://cn.pingcap.com/blog/observability-of-distributed-system/)中的可视化方式
![TPC-C](https://img1.www.pingcap.com/prod/2_TPC_C_da4fbe5809.png)

图中横轴是时间，纵轴是数据的分布。
通过这样的带有时间轴的热力图可以观察到在一个线性的地址空间中，IO 的范围随着时间变化的模式，可以对 IO Pattern 有一个比较直观的认识，方便判断是否存在业务本身对 ZBS 不友好的情况，进一步帮助寻找调优方案。
由于 IO 是通过 Volume 的逻辑地址映射到磁盘上，在 Extent 内部是大致连续的，Volume 的逻辑地址上的 IO 出现热点，那么磁盘必然出现热点，当然这个场景下我们并不关心热点的具体位置，而是关心其 IO压力数值、时间范围、变化趋势。
## 磁盘级别的 IO 负载情况
如果不考虑 IO Pattern 仅仅关注磁盘负载的问题，目前实际上已经有节点级别的 metric 可以实现，增加一些专门用于判定磁盘负载不均衡的预设的监控视图实际上就足够帮助发现此类问题。
同时在离线分析的过程中，借助监控数据判定磁盘高负载的情况。
## 数据恢复
在集群中的所有 Chunk 之间存在着数据恢复的过程，数据恢复目前我们是限制速度的，一般不会造成性能问题，仅有一个 [case]( https://cs.smartx.com/cases/detail?id=1044211)  显示因为 Recover 不均衡导致卡住, 但是总体而言 Recover 是 ZBS 运行过程中的一个比较重要的过程，有必要使得这个过程对用户更透明。
数据恢复的可视化部分主要展示恢复在各个节点的积累量，正在进行的恢复的流量分布情况等数据，目标是让用户直观感受到目前恢复进行的状态以及未来将要发生的恢复的状态。
# Volume 数据分布
展示 volume 的数据在节点的分布情况，可以判定其是否失去本地化，能让用户更直观更清晰了解到数据目前存放在何处，数据当前的安全状况如何。


# 产品形态
当前阶段有两个目标方向：
1. 提升 ZBS 的可观测性，让 ZBS 的实时运行状态能够更清晰地展示出来，在定位性能问题的时候能够快速将问题的范围缩小；这个功能的实现形式是一些实时的监控图表，按照具体情况可以单独实现，也可以通过预设一些监控视图来实现。
2. 一键诊断可能影响性能的环境问题和配置问题，这个功能的实现形式是一个 Check List， 主要将我们已知的风险点进行逐一排查，对有问题的项目给出提示和建议的修复方式。




## RPC 负载情况
每个服务上的 RPC 负载情况目前没有一个比较方便的展示方式，应该增加 Metric 以展示 ZBS 内部的 RPC 的运行状况



SLOW IO Trace
```bash
W1106 10:19:14.214831 11910 access_io_handler.cc:545] [ACCESS SLOW IO TRACE] volume_id: "6a6822f0-285d-41e1-8535-ba59027b5ecb", vextent no: 296, data_len: 4096, extent_offset: 162148352, flags: 0, gen: 13211, loc: 5 , elapsed_ms: 901, expect_event_num: 8
{ ACCESS_IO_BEGIN: rel_time_us=0 }
{ ACCESS_SYNC_GEN: rel_time_us=0 }
{ ACCESS_WAIT_RECOVER: rel_time_us=754016 }
{ ACCESS_REPLICA_IO_BEGIN: rel_time_us=754016 cid=5 data_len=4096 extent_offset=162148352 flags=0 gen=13211 op=3 pid=6928481 }
{ ACCESS_REPLICA_IO_BEGIN: rel_time_us=754016 cid=3 data_len=4096 extent_offset=162148352 flags=0 gen=13211 op=3 pid=6928481 }
{ ACCESS_REPLICA_IO_END: rel_time_us=755016 cid=5 error_code=0 op=3 pid=6928481 }
{ ACCESS_REPLICA_IO_END: rel_time_us=795017 cid=3 error_code=0 op=3 pid=6928481 }
{ ACCESS_IO_END: rel_time_us=897019 error_code=0 }
```

```bash
W1106 10:22:18.600709 11910 zbs_client.cc:1118] [CLIENT SLOW IO TRACE] [IOCTX]: VEXTENT_WRITE seq: 61887, volume: "419efeb5-6d2a-44e7-88dc-5dab684985d5", in_progress: 1, volume_offset: 43828158464, len: 4096, return_not_alloc: 0, resize: 0, retry_times: 0, next_retry_ms: 0 , vextent no: 163, data_len: 4096, extent_offset: 219455488, pid: 6918837, owner_ip: "10.0.67.99", owner_port: 10201 , elapsed_ms: 3608, expect_event_num: 2
```

```bash

```

展示一些关键属性是否开启：LSM 多线程，最好展示全部的 gflags

每个端口是否能服务，应该检测

[Grafana](https://ce-demo.deepflow.yunshan.net/dashboards/f/ld6wH0g4k/deepflow-templates)
[Huawei OceanStore Storage | Grafana Labs](https://grafana.com/grafana/dashboards/2695-huawei-storage/)
[Huawei Dorado Storage | Grafana Labs](https://grafana.com/grafana/dashboards/4329-huawei-storage/)


ZBS Overview: 这里面主要是一些ZBS运行的历史状态的metric
	主要包含： meta和chunk 的内存/CPU占用，集群的IO压力，Meta 的角色变化（主要观察Leader切换），Cache Miss 概率和落在容量层的IO（对性能影响大），迁移，主机之间的网络延迟
Real Time System Status：这里面展示 zbs 实时的系统压力
Disk： 展示物理磁盘的压力
Abnormal Detect：展示出现异常压力/性能差的时间段


缺乏的 Tag 还需要多打点，比如 Chunk 的必须包含 Chunk ID
Volume 需要加一个统计量

也许应该做在 Alpha 插件

异常检测[Practical Anomaly Detection | Prometheus](https://prometheus.io/blog/2015/06/18/practical-anomaly-detection/)

如何分析日志里面的事件流
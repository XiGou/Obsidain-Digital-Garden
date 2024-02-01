当前阶段的主要目标：辅助定位性能问题
## Migrate & Recover
有部分 Case 是由于 Migrate & Recover 卡住导致系统异常，从 dogfood 来看，这两者的速度值偶尔飙升，然后归零，Pending 数据量均为忽然飙升，然后缓慢下降归零；部分会忽然归零， 原因应该是在观察的时间段内没有真的发生磁盘丢失引发完整的数据恢复流程，而是节点短暂地下线后又快速上线。
同时 Migrate 的 Pending 值在观察的时间窗口即使有 Migrate 速度也为 0。可能是这个 metric根本没起作用或者更新不及时。
但是总体来讲，如果发生卡死，应当明显观察到 Pending 值长期保持非零值。


[Site Unreachable](https://docs.google.com/document/d/1YV0p_vyOMJrnosFrh7Oo5_hFJpeQ1bSRJh8Kr0k0gfo/edit#heading=h.wfhi0u4x2tzo)


## HDD 性能
HDD 击穿是导致 ZBS 性能问题的一个重要原因，击穿主要从两个角度来看：
1. 真正落到 HDD 的 IOPS 和 BW ，从 ZBS 内部的 Metric（Cache Hit Rate 、带宽、IOPS）可以计算出来
2. 从 OS 的 exporter 观测到 HDD 实际的 Lat、BW、IOPS 信息
3. 落到 HDD 的 IO Pattern
其中 1、2 容易取得，而 3 实际上难以获取，需要通过 Trace 技术才能获得最准确的信息，通过 Metric 也可以做一定的实现，可在 LSM 层增加针对每个 Disk 级别的 IO Pattern 汇总 Metric 暴露出来，此处会增加 `磁盘数量*指标数量` 个时间序列
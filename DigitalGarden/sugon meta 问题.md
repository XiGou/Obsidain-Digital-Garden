iscsi-redirectord

```
[Switching to thread 1 (LWP 3850100)]
#0  0x00007f42357f046b in raise () from /lib64/libc.so.6
(gdb) bt
#0  0x00007f42357f046b in raise () from /lib64/libc.so.6
#1  0x00007f42357f1791 in abort () from /lib64/libc.so.6
#2  0x0000556e19791544 in google::DumpStackTraceAndExit () at glog/src/utilities.cc:152
#3  0x0000556e19786e6d in google::LogMessage::Fail () at glog/src/logging.cc:1488
#4  0x0000556e1978ae5a in google::LogMessage::SendToLog (this=0x556e290c8b10) at glog/src/logging.cc:1442
#5  0x0000556e19786900 in google::LogMessage::Flush (this=0x556e290c8b10) at glog/src/logging.cc:1311
#6  0x0000556e19788399 in google::LogMessageFatal::~LogMessageFatal (this=<optimized out>, __in_chrg=<optimized out>) at glog/src/logging.cc:2023
#7  0x0000556e1948c189 in zbs::AsyncBatchHandleQueueImpl<zbs::consensus::ZooKeeper::ZkWatchContext*, true, std::function<void (std::deque<zbs::consensus::ZooKeeper::ZkWatchContext*, std::allocator<zbs::consensus::ZooKeeper::ZkWatchContext*> >&&)>, std::function<void (zbs::consensus::ZooKeeper::ZkWatchContext* const&)> >::AsyncBatchHandleQueueImpl(zbs::EventLoop*, std::function<void (std::deque<zbs::consensus::ZooKeeper::ZkWatchContext*, std::allocator<zbs::consensus::ZooKeeper::ZkWatchContext*> >&&)> const&) (this=<optimized out>, loop=0x556e1bf140c0, event_batch_handler=...) at src/common/async_queue.h:37
#8  0x0000556e1948c2c2 in zbs::AsyncQueue<zbs::consensus::ZooKeeper::ZkWatchContext*, zbs::ExecInCoroutine, std::function<void (zbs::consensus::ZooKeeper::ZkWatchContext* const&)> >::AsyncQueue(zbs::EventLoop*, std::function<void (zbs::consensus::ZooKeeper::ZkWatchContext* const&)> const&) (event_handler=..., 
    loop=<optimized out>, this=0x556e290c9138) at src/common/async_queue.h:250
#9  zbs::consensus::ZooKeeper::ZooKeeper (this=0x556e290c9100, zoo_hosts="172.21.30.1:2181", zk_digest="", loop=<optimized out>) at src/consensus/zookeeper.cc:121
#10 0x0000556e194868af in zbs::consensus::GetAliveMembersFromZk (loop=0x556e1bf140c0, zk_hosts="172.21.30.1:2181", service_name="meta", 
    members=members@entry=0x556e290c9380) at src/consensus/zookeeper_util.cc:104
#11 0x0000556e19487ea2 in zbs::consensus::GetLeaderFromZk (loop=<optimized out>, zk_hosts="172.21.30.1:2181", service_name="meta", 
    leader=leader@entry=0x556e290c94f0) at src/consensus/zookeeper_util.cc:148
#12 0x0000556e194a10da in zbs::libmeta::ZkMetaLocator::GetMetaAddress (this=<optimized out>, ip=0x556e290c9720, port=0x556e290c970a)
    at src/libmeta/meta_locator.cc:24
#13 0x0000556e194ad4d0 in zbs::libzbs::MetaClient::RefreshMetaConnection (this=this@entry=0x556e1be78840) at src/libzbs/meta_client.cc:107
#14 0x0000556e197ded0b in zbs::libzbs::MetaClient::CallMethod<bool, std::vector<zbs::meta::ISCSITarget, std::allocator<zbs::meta::ISCSITarget> >*, bool, std::vector<zbs::meta::ISCSITarget, std::allocator<zbs::meta::ISCSITarget> >*> (this=0x556e1be78840, method=<optimized out>, argstr=0x556e198772aa "false, &meta_targets", 
    method_name=0x556e198772bf "ListTargets", ctrl=0x0, retry_times=5) at src/libzbs/meta_client.h:85
#15 0x0000556e197e0c5b in zbs::iscsi::TargetsManager::ListTargets (this=0x556e1bf18040, targets=targets@entry=0x556e290c9b30, 
    initiator_iqn="iqn.1994-05.com.redhat:9ab652ef185c", initiator_addr="172.22.36.92") at src/iscsi_redirector/targets_manager.cc:52
#16 0x0000556e197ebbb7 in zbs::iscsi::ISCSIConnection::HandleNormalSession (this=this@entry=0x556e28b02000) at src/iscsi_redirector/connection.cc:1001
#17 0x0000556e197ed255 in zbs::iscsi::ISCSIConnection::Routine (this=0x556e28b02000) at src/iscsi_redirector/connection.cc:1071
#18 0x0000556e197ed453 in zbs::iscsi::ISCSIConnection::<lambda()>::operator() (__closure=<optimized out>) at src/iscsi_redirector/connection.cc:82
#19 zbs::CoFunc::Impl<zbs::iscsi::ISCSIConnection::ISCSIConnection(zbs::EventLoop*, zbs::iscsi::TargetsManager*, zbs::SocketPtr, uint16_t, std::function<void(zbs::iscsi::ISCSIConnection*)>)::<lambda()> >::s_run_and_reset(zbs::CoFunc::storage_t &) (storage=...) at src/common/coroutine.h:276
#20 0x0000556e194db6d9 in zbs::CoFunc::run_and_reset (this=0x556e1c6cd630) at src/common/coroutine.h:312
#21 zbs::Coroutine::LoopMain (this=0x556e1c6cd520) at src/common/coroutine.cc:615
#22 0x0000556e194db752 in zbs::Coroutine::s_co_main (lo=<optimized out>, hi=<optimized out>) at src/common/coroutine.cc:602
```

E0809 10:56:20.255676 517547 lun.cc:262] cmd: 0x9e SENSE KEY: SPDK_SCSI_SENSE_ILLEGAL_REQUEST, ASCQ:LOGICAL_UNIT_NOT_SUPPORTED
E0809 10:56:20.255839 517547 lun.cc:262] cmd: 0x25 SENSE KEY: SPDK_SCSI_SENSE_ILLEGAL_REQUEST, ASCQ:LOGICAL_UNIT_NOT_SUPPORTED
E0809 10:56:20.255964 517547 lun.cc:262] cmd: 0x1a SENSE KEY: SPDK_SCSI_SENSE_ILLEGAL_REQUEST, ASCQ:LOGICAL_UNIT_NOT_SUPPORTED



#0  0x00007fab4d66d46b in raise () from /lib64/libc.so.6
#1  0x00007fab4d66e791 in abort () from /lib64/libc.so.6
#2  0x000055cd01a4d544 in google::DumpStackTraceAndExit () at glog/src/utilities.cc:152
#3  0x000055cd01a42e6d in google::LogMessage::Fail () at glog/src/logging.cc:1488
#4  0x000055cd01a46e5a in google::LogMessage::SendToLog (this=0x55cd0e0cc7f0) at glog/src/logging.cc:1442
#5  0x000055cd01a42900 in google::LogMessage::Flush (this=0x55cd0e0cc7f0) at glog/src/logging.cc:1311
#6  0x000055cd01a44399 in google::LogMessageFatal::~LogMessageFatal (this=<optimized out>, __in_chrg=<optimized out>) at glog/src/logging.cc:2023
#7  0x000055cd01748189 in zbs::AsyncBatchHandleQueueImpl<zbs::consensus::ZooKeeper::ZkWatchContext*, true, std::function<void (std::deque<zbs::consensus::ZooKeeper::ZkWatchContext*, std::allocator<zbs::consensus::ZooKeeper::ZkWatchContext*> >&&)>, std::function<void (zbs::consensus::ZooKeeper::ZkWatchContext* const&)> >::AsyncBatchHandleQueueImpl(zbs::EventLoop*, std::function<void (std::deque<zbs::consensus::ZooKeeper::ZkWatchContext*, std::allocator<zbs::consensus::ZooKeeper::ZkWatchContext*> >&&)> const&) (this=<optimized out>, loop=0x55cd034080c0, event_batch_handler=...)
    at src/common/async_queue.h:37
#8  0x000055cd017482c2 in zbs::AsyncQueue<zbs::consensus::ZooKeeper::ZkWatchContext*, zbs::ExecInCoroutine, std::function<void (zbs::consensus::ZooKeeper::ZkWatchContext* const&)> >::AsyncQueue(zbs::EventLoop*, std::function<void (zbs::consensus::ZooKeeper::ZkWatchContext* const&)> const&) (
    event_handler=..., loop=<optimized out>, this=0x55cd0e0cce18) at src/common/async_queue.h:250
#9  zbs::consensus::ZooKeeper::ZooKeeper (this=0x55cd0e0ccde0, zoo_hosts="172.21.30.2:2181", zk_digest="", loop=<optimized out>)
    at src/consensus/zookeeper.cc:121
#10 0x000055cd017428af in zbs::consensus::GetAliveMembersFromZk (loop=0x55cd034080c0, zk_hosts="172.21.30.2:2181", service_name="meta",
    members=members@entry=0x55cd0e0cd060) at src/consensus/zookeeper_util.cc:104
#11 0x000055cd01743ea2 in zbs::consensus::GetLeaderFromZk (loop=<optimized out>, zk_hosts="172.21.30.2:2181", service_name="meta",
    leader=leader@entry=0x55cd0e0cd1d0) at src/consensus/zookeeper_util.cc:148
#12 0x000055cd0175d0da in zbs::libmeta::ZkMetaLocator::GetMetaAddress (this=<optimized out>, ip=0x55cd0e0cd400, port=0x55cd0e0cd3ea)
    at src/libmeta/meta_locator.cc:24
#13 0x000055cd017694d0 in zbs::libzbs::MetaClient::RefreshMetaConnection (this=this@entry=0x55cd0336c840) at src/libzbs/meta_client.cc:107
#14 0x000055cd01a9d3a4 in zbs::libzbs::MetaClient::CallMethod<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*> (method=<optimized out>,
    argstr=0x55cd01b33368 "initiator_name, short_target_name, initiator_ip, server_ip, FLAGS_local_access_server_addr, &portal",
    method_name=0x55cd01b33340 "GetServicePortal", ctrl=0x0, retry_times=5, this=0x55cd0336c840) at src/libzbs/meta_client.h:85
#15 zbs::iscsi::TargetsManager::GetTargetPortal (this=this@entry=0x55cd0340c040, initiator_name="iqn.1994-05.com.redhat:2adc7190fe", target_name=...,
    initiator_addr="172.22.33.77:51758", server_addr="172.22.30.100:3260") at src/iscsi_redirector/targets_manager.cc:130
#16 0x000055cd01aa5b02 in zbs::iscsi::ISCSIConnection::DoRedirection (this=this@entry=0x55cd089bd800, login_target=...)
    at src/iscsi_redirector/connection.cc:751
#17 0x000055cd01aa8085 in zbs::iscsi::ISCSIConnection::HandleNormalSession (this=this@entry=0x55cd089bd800) at src/iscsi_redirector/connection.cc:1026
#18 0x000055cd01aa9255 in zbs::iscsi::ISCSIConnection::Routine (this=0x55cd089bd800) at src/iscsi_redirector/connection.cc:1071
#19 0x000055cd01aa9453 in zbs::iscsi::ISCSIConnection::<lambda()>::operator() (__closure=<optimized out>) at src/iscsi_redirector/connection.cc:82
#20 zbs::CoFunc::Impl<zbs::iscsi::ISCSIConnection::ISCSIConnection(zbs::EventLoop*, zbs::iscsi::TargetsManager*, zbs::SocketPtr, uint16_t, std::function<void(zbs::iscsi::ISCSIConnection*)>)::<lambda()> >::s_run_and_reset(zbs::CoFunc::storage_t &) (storage=...) at src/common/coroutine.h:276
--Type <RET> for more, q to quit, c to continue without paging--Quit
这个程序为什么会崩溃

Claude
APP  10:47 AM
根据崩溃的调用栈信息,这个程序出现崩溃的主要原因是:
1. 在zbs::iscsi::ISCSIConnection::DoRedirection函数中,调用了zbs::iscsi::TargetsManager::GetTargetPortal函数获取目标portal信息。
2. 在zbs::iscsi::TargetsManager::GetTargetPortal中,调用了zbs::libzbs::MetaClient的RPC方法GetServicePortal去元数据服务获取portal信息。
3. 在执行GetServicePortal时,发生了RPC失败或者超时,导致获取不到预期的portal信息。
4. 由于获取不到portal,后续逻辑无法继续,但没有进行错误处理,直接使用了空或非法的参数,导致崩溃。
5. 崩溃最后抛出在google日志模块的CHECK函数中,造成了程序的异常退出。
总结起来,主要是调用链路上对网络RPC失败或者超时的错误处理不够完善,直接使用了非法参数,导致后续逻辑崩溃。
需要在调用MetaClient的地方对RPC失败进行判断处理,返回特定错误,避免使用非法参数。并在DoRedirection等函数中判断错误,进行失败处理,防止错误继续传递导致崩溃。
此外,也需要检查MetaClient和元数据服务本身的稳定性,尽量避免RPC请求失败。 (edited) 



ystem-zbs-metamain.slice    system-zbs-metamain.slice.d/
[root@zbs03 17:26:34 zbs]$vi /etc/systemd/system/system-zbs-metamain.slice.d/50-MemoryLimit.conf
[root@zbs03 17:27:34 zbs]$python
Python 2.7.18 (default, Dec 31 2021, 19:58:09)
[GCC 7.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 19*1024**3
20401094656
>>> 9*1024**3
9663676416
>>> quit()
[root@zbs03 17:28:33 zbs]$cat /etc/systemd/system/system-zbs-metamain.slice.d/50-MemoryLimit.conf
[Slice]
MemoryLimit=9663676416
[root@zbs03 17:28:37 zbs]$20401094656
[root@zbs03 17:28:49 zbs]$v  /etc/systemd/system/system-zbs-metamain.slice.d/50-MemoryLimit.conf
bash: v: command not found
[root@zbs03 17:28:55 zbs]$vi  /etc/systemd/system/system-zbs-metamain.slice.d/50-MemoryLimit.conf
[root@zbs03 17:29:28 zbs]$systemctl daemon-reloadsystemctl daemon-reload
Unknown operation daemon-reloadsystemctl.
[root@zbs03 17:29:38 zbs]$cat  /etc/systemd/system/system-zbs.slice.d/50-MemoryLimit.conf
[Slice]
MemoryLimit=35433480192
[root@zbs03 17:29:59 zbs]$python
Python 2.7.18 (default, Dec 31 2021, 19:58:09)
[GCC 7.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 35433480192 /(1024**3)
33
>>> quit()
[root@zbs03 17:30:16 zbs]$python
Python 2.7.18 (default, Dec 31 2021, 19:58:09)
[GCC 7.3.0] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> 43*(1024**#)
...
KeyboardInterrupt
>>> 43*1024**3
46170898432
>>> quit()
[root@zbs03 17:30:53 zbs]$vi  /etc/systemd/system/system-zbs.slice.d/50-MemoryLimit.conf
[root@zbs03 17:31:15 zbs]$vi  /etc/systemd/system/system-zbs.slice.d/50-MemoryLimit.conf
[root@zbs03 17:31:30 zbs]$systemctl daemon-reloadsystemctl daemon-reload
[root@zbs03 17:31:46 zbs]$systemctl daemon-reload
[root@zbs03 17:31:49 zbs]$systemctl set-property --runtime system-zbs.slice MemoryLimit=systemctl set-property --runtime system-zbs.slice MemoryLimit=
[root@zbs03 17:32:01 zbs]$systemctl set-property --runtime system-zbs.slice MemoryLimit=46170898432
[root@zbs03 17:32:08 zbs]$4617089843246170898432
[root@zbs03 17:32:25 zbs]$systemctl set-property --runtime zbs-chunkd.service MemoryLimitsystemctl set-property --runtime zbs-chunkd.service MemoryLimit
[root@zbs03 17:32:39 zbs]$systemctl set-property --runtime zbs-metad.service MemoryLimit=20401094656
[root@zbs03 17:34:03 zbs]$systemctl restart zbs-metad
Warning: The unit file, source configuration file or drop-ins of zbs-metad.service changed on disk. Run 'systemctl daemon-reload' to reload units.
[root@zbs03 17:34:25 zbs]$systemctl daemon-reload
[root@zbs03 17:34:33 zbs]$systemctl restart zbs-metad
[root@zbs03 17:34:36 zbs]$systemctl status zbs-metad
 
vi /etc/systemd/system/system-zbs-metamain.slice.d/50-MemoryLimit.conf
20401094656

46170898432
systemctl set-property --runtime system-zbs.slice MemoryLimit=46170898432
systemctl set-property --runtime zbs-metad.service MemoryLimit=20401094656



 target list | awk 'NR>2 {print $2}' > target_names
cat target_names | xargs -I {} zbs-iscsi target delete {} --recursive true
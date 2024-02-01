
sudo zbs-meta -v  pextent getref 52803
2023-11-29 16:50:27,320, ERROR, cmd.py:2866, Traceback:
ErrorCode [ENotFound] NotFound: fail to get dd3815ab-7e96-4198-a598-8adfec8e98d8 from db /var/lib/zbs/metad/db//meta/a Volume dd3815ab-7e96-4198-a598-8adfec8e98d8 is not found.
UserCode [UVolumeNotFound]
Traceback (most recent call last):
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/meta/cmd.py", line 2859, in main
    parser.run()
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/lib/parser.py", line 53, in run
    self.args.func(self.args)
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/meta/cmd.py", line 1110, in pextent_getref
    response = client.pextent_getref(args.pextent_id)
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/meta/client.py", line 112, in pextent_getref
    return ProtoStrWrap(self.meta_service.GetPExtentRef(None, request, None))
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/google/protobuf/service_reflection.py", line 278, in <lambda>
    self._StubMethod(inst, method, rpc_controller, request, callback))
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/google/protobuf/service_reflection.py", line 293, in _StubMethod
    return stub.rpc_channel.CallMethod(
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/lib/rpc/channel.py", line 226, in CallMethod
    raise ZException(response)
zbs.lib.zexception.ZException: Traceback:
[ENotFound] NotFound: fail to get dd3815ab-7e96-4198-a598-8adfec8e98d8 from db /var/lib/zbs/metad/db//meta/a Volume dd3815ab-7e96-4198-a598-8adfec8e98d8 is not found.

E1129 16:50:27.318619 18742 meta_rpc_server.cc:2988] failed to find extent ref for pid: 52803 volume id: dd3815ab-7e96-4198-a598-8adfec8e98d8 st: [UVolumeNotFound]
Traceback:
[ENotFound]: NotFound: fail to get dd3815ab-7e96-4198-a598-8adfec8e98d8 from db /var/lib/zbs/metad/db//meta/a Volume dd3815ab-7e96-4198-a598-8adfec8e98d8 is not found.


GetPExtentRef 失败 
meta 日志：
```
E1129 16:50:27.318619 18742 meta_rpc_server.cc:2988] failed to find extent ref for pid: 52803 volume id: dd3815ab-7e96-4198-a598-8adfec8e98d8 st: [UVolumeNotFound]
```
这个RPC 会遍历vtable，但是通过dd3815ab-7e96-4198-a598-8adfec8e98d8  无法从metaDB获得这个 Volume，是内存中的 vtable 和DB 中的 vtable不一致


[smartx@Node-172-20-128-104 17:02:10 ~]$ sudo zbs-meta volume listall | grep dd3815ab-7e96-4198-a598-8adfec8e98d8
[smartx@Node-172-20-128-104 17:02:16 ~]$ sudo zbs-meta volume listall | grep ^C
[smartx@Node-172-20-128-104 17:02:31 ~]$ 


[smartx@Node-172-20-128-106 17:43:50 ~]$ sudo zbs-meta -v  pextent show  52804
--------------------  ----------
ID                    52804
Replica               [3]
Alive Replica         []
Temporary Replica     []
Ever Exist            True
Is Garbage            False
Origin PExtent        0
Expected Replica num  1
Epoch                 34639405
Prefer Local          1
Allocated Space       256.00 MiB
--------------------  ----------

[smartx@Node-172-20-128-106 18:10:07 ~]$ sudo grep -A 3 "DELETE VOLUME" /var/log/zbs/zbs-metad* 
/var/log/zbs/zbs-metad.log.20231116-161847.16928:I1117 17:42:30.469712 32306 meta_rpc_server.cc:1410] [DELETE VOLUME]: [REQUEST]: pool_path { pool_name: "default" } volume_name: "volume_2987", [RESPONSE]: ST:OK, , [TIME]: 5178 us.

[smartx@Node-172-20-128-106 18:10:29 ~]$ sudo grep  "volume_2987" /var/log/zbs/zbs-metad* 
/var/log/zbs/zbs-metad.log.20231116-161847.16928:I1117 17:42:26.930879 32306 meta_rpc_server.cc:548] [CREATE VOLUME]: [REQUEST]: pool_path { pool_name: "default" } volume { name: "volume_2987" size: 1073741824 read_only: false }, [RESPONSE]: ST:OK, name: "volume_2987" size: 1073741824 created_time { seconds: 1700214146 nseconds: 926928041 } id: "62829a00-50ee-4a1c-97f3-acaf95458c61" parent_id: "5931e16d-7da1-4544-9fc0-2683282d0a07" replica_num: 2 thin_provision: true read_only: false iops_burst: 0 bps_burst: 0 throttling { } stripe_num: 1 stripe_size: 262144, [TIME]: 4076 us.
/var/log/zbs/zbs-metad.log.20231116-161847.16928:I1117 17:42:28.230350 32306 meta_rpc_server.cc:1057] [ALLOC PEXTENT]: pid: 432646 volume: name: "volume_2987" size: 1073741824 created_time { seconds: 1700214146 nseconds: 926928041 } id: "62829a00-50ee-4a1c-97f3-acaf95458c61" parent_id: "5931e16d-7da1-4544-9fc0-2683282d0a07" replica_num: 2 thin_provision: true read_only: false iops_burst: 0 bps_burst: 0 throttling { } stripe_num: 1 stripe_size: 262144 loc: [3 4 ]
/var/log/zbs/zbs-metad.log.20231116-161847.16928:I1117 17:42:28.613057 32306 meta_rpc_server.cc:1057] [ALLOC PEXTENT]: pid: 432647 volume: name: "volume_2987" size: 1073741824 created_time { seconds: 1700214146 nseconds: 926928041 } id: "62829a00-50ee-4a1c-97f3-acaf95458c61" parent_id: "5931e16d-7da1-4544-9fc0-2683282d0a07" replica_num: 2 thin_provision: true read_only: false iops_burst: 0 bps_burst: 0 throttling { } stripe_num: 1 stripe_size: 262144 loc: [3 4 ]
/var/log/zbs/zbs-metad.log.20231116-161847.16928:I1117 17:42:28.945459 32306 meta_rpc_server.cc:1057] [ALLOC PEXTENT]: pid: 432648 volume: name: "volume_2987" size: 1073741824 created_time { seconds: 1700214146 nseconds: 926928041 } id: "62829a00-50ee-4a1c-97f3-acaf95458c61" parent_id: "5931e16d-7da1-4544-9fc0-2683282d0a07" replica_num: 2 thin_provision: true read_only: false iops_burst: 0 bps_burst: 0 throttling { } stripe_num: 1 stripe_size: 262144 loc: [3 4 ]
/var/log/zbs/zbs-metad.log.20231116-161847.16928:I1117 17:42:29.395460 32306 meta_rpc_server.cc:1057] [ALLOC PEXTENT]: pid: 432649 volume: name: "volume_2987" size: 1073741824 created_time { seconds: 1700214146 nseconds: 926928041 } id: "62829a00-50ee-4a1c-97f3-acaf95458c61" parent_id: "5931e16d-7da1-4544-9fc0-2683282d0a07" replica_num: 2 thin_provision: true read_only: false iops_burst: 0 bps_burst: 0 throttling { } stripe_num: 1 stripe_size: 262144 loc: [3 4 ]
/var/log/zbs/zbs-metad.log.20231116-161847.16928:I1117 17:42:30.468999 32306 meta_rpc_server.cc:1484] A volume is deleted. pool: 5931e16d-7da1-4544-9fc0-2683282d0a07 volume: volume_2987
/var/log/zbs/zbs-metad.log.20231116-161847.16928:I1117 17:42:30.469712 32306 meta_rpc_server.cc:1410] [DELETE VOLUME]: [REQUEST]: pool_path { pool_name: "default" } volume_name: "volume_2987", [RESPONSE]: ST:OK, , [TIME]: 5178 us.



/var/log/zbs/zbs-chunkd.log.20231125-101029.30877:I1125 11:09:45.253851 30883 access_io_handler.cc:1749] [GET TEMPORARY REPLICA GENERATION] pid: 1167677 epoch: 32219471 temporary_pid: 52804 temporary_epoch: 34639405

/var/log/zbs/zbs-chunkd.log.20231125-101029.30877:I1125 11:11:01.785198 30883 recover_handler.cc:313] Get recover notification: pid: 1167677 lease { owner { uuid: "a35f1b4d-97ca-46a8-8f3e-cb110fccb07b" ip: "10.200.0.105" num_ip: 1761658890 port: 10201 cid: 1 secondary_data_ip: "172.20.128.105" zone: "default" scvm_mode_host_data_ip: "" alive_sec: 180 machine_uuid: "84dbbb60-7db2-11ed-9dce-525400ce232b" } pid: 1167677 location: 1 origin_pid: 0 epoch: 32219471 origin_epoch: 0 ever_exist: true meta_generation: 13243 expected_replica_num: 2 thin_provision: true chunks { id: 1 data_ip: 1761658890 data_port: 10201 rpc_ip: 1761658890 rpc_port: 10200 zone_id: "default" } temporary_replicas { temporary_pid: 52804 temporary_epoch: 34639405 failed_cid: 5 create_at_generation: 13243 location: 3 ever_exist: true meta_generation: 1 pid: 1167677 epoch: 32219471 lossy: false } prioritized: false } dst_chunk: 5 src_chunk: 1 epoch: 32219471 active_location: 1 agile_recover_only: true start_ms: 8691481497



[smartx@Node-172-20-128-105 18:25:05 ~]$ sudo zbs-meta pextent getref 51629
2023-11-29 18:25:26,610, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:25:26 ~]$ sudo zbs-meta pextent getref 51644
2023-11-29 18:25:31,739, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:25:31 ~]$ sudo zbs-meta pextent getref 51660
2023-11-29 18:25:36,256, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:25:36 ~]$ sudo zbs-meta pextent getref 51703
2023-11-29 18:25:40,798, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:25:40 ~]$ sudo zbs-meta pextent getref 51735
2023-11-29 18:25:45,242, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:25:45 ~]$ sudo zbs-meta pextent getref 51898
2023-11-29 18:25:50,278, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:25:50 ~]$ sudo zbs-meta pextent getref 51915
2023-11-29 18:25:55,017, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:25:55 ~]$ sudo zbs-meta pextent getref 51926
2023-11-29 18:26:00,753, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:26:00 ~]$ sudo zbs-meta pextent getref 51960
2023-11-29 18:26:05,585, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:26:05 ~]$ sudo zbs-meta pextent getref 51964
2023-11-29 18:26:09,786, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d from db /var/lib/zbs/metad/db//meta/a Volume 7528c9bb-eaa3-4236-8fb3-a857cdf73d3d is not found.
[smartx@Node-172-20-128-105 18:26:09 ~]$ sudo zbs-meta pextent getref 52803
2023-11-29 18:26:13,841, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get dd3815ab-7e96-4198-a598-8adfec8e98d8 from db /var/lib/zbs/metad/db//meta/a Volume dd3815ab-7e96-4198-a598-8adfec8e98d8 is not found.
[smartx@Node-172-20-128-105 18:26:14 ~]$ sudo zbs-meta pextent getref 52804
2023-11-29 18:26:18,643, ERROR, cmd.py:2868, Traceback:
[ENotFound] NotFound: fail to get dd3815ab-7e96-4198-a598-8adfec8e98d8 from db /var/lib/zbs/metad/db//meta/a Volume dd3815ab-7e96-4198-a598-8adfec8e98d8 is not found.



[smartx@Node-172-20-128-106 11:07:56 ~]$ rpm -qa | grep zbs
zbs-5.5.1-rc18.0.release.git.g38cc80ec0.el7.SMTX.HCI.x86_64
libzbs-5.5.1-rc18.0.release.git.g38cc80ec0.el7.SMTX.HCI.x86_64
zbs-perf-tools-1.0.1-rc7.0.ge1c6b00.el7.centos.x86_64
pyzbs-6.0.0-rc114.0.git.g8c1d7bbc6.el7.centos.x86_64
zbs-client-py3-5.5.1-rc7.0.el7.x86_64
zbs-debuginfo-5.5.1-rc9.0.release.git.g1799fb8eb.el7.SMTX.HCI.x86_64


[smartx@Node-172-20-128-106 11:10:15 ~]$ sudo gcore -o zbs-metad-5.5.1-rc18.0.release.git.g38cc80ec0 19262
warning: the debug information found in "/usr/lib/debug//usr/sbin/zbs-metad.debug" does not match "/usr/sbin/zbs-metad" (CRC mismatch).

warning: the debug information found in "/usr/lib/debug/usr/sbin/zbs-metad.debug" does not match "/usr/sbin/zbs-metad" (CRC mismatch).

[New LWP 26489]
[New LWP 26488]
[New LWP 26487]
[New LWP 26455]
[New LWP 26451]
[New LWP 26209]
[New LWP 26208]
[New LWP 26207]
[New LWP 18745]
[New LWP 18744]
[New LWP 18743]
[New LWP 18742]
[New LWP 19321]
[New LWP 19279]
[New LWP 19269]
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib64/libthread_db.so.1".
0x00007fabb8f411ad in nanosleep () from /lib64/libc.so.6
warning: target file /proc/19262/cmdline contained unexpected null characters
Saved corefile zbs-metad-5.5.1-rc18.0.release.git.g38cc80ec0.19262


dd3815ab-7e96-4198-a598-8adfec8e98d8 线索

 ssh 10.200.0.107 zgrep dd3815ab-7e96-4198-a598-8adfec8e98d8 /var/log/zbs/zbs-*
/var/log/zbs/zbs-chunkd.log.20230515-184641.13570.gz:id: "dd3815ab-7e96-4198-a598-8adfec8e98d8"

D\xce
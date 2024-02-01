虚拟机 I/O 平均延时，高 IO 延迟有两个时间段，12:52 - 12:58。13:08 - 13:14


[smartx@elf-172-20-128-83 16:07:16 ~]$ 

W1108 13:08:19.048750  2205 vhost_scsi.c:369] [SPDK/vhost_scsi_task_cpl] request CDB
00000000  37 00 0c 00 00 00 00 00  04 00 00 00 00 00 00 00  7...............
00000010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00   ................
E1108 13:08:19.076159 11910 access_io_handler.cc:691] [FAIL TO READ VEXTENT]: req: VEXTENT_READ version: 4097, error_code: 0, op: 0, buf_len: 0, message_id: 5472223 , volume_id: "5e30543e-0967-4a48-b0ff-ff4652dd7485", vextent no: 15, data_len: 122880, extent_offset: 106254336, flags: 0  st:
Traceback:
[ENotOwner]:  current session: cc8207a7-fa1f-4527-8c53-0d5478528472 lease: owner { uuid: "039e3854-348a-4ddf-aa7a-4cf2c6150dd7" ip: "10.0.67.87" num_ip: 1464008714 port: 10201 cid: 3 secondary_data_ip: "172.20.128.81" zone: "default" scvm_mode_host_data_ip: "" alive_sec: 937 machine_uuid: "513f7ae2-d4e5-11ec-9890-b4055d3205f8" } pid: 6974125 location: 769 origin_pid: 5146917 epoch: 8396020 origin_epoch: 6123689 ever_exist: true meta_generation: 1 expected_replica_num: 2 thin_provision: true chunks { id: 3 data_ip: 1464008714 data_port: 10201 rpc_ip: 1464008714 rpc_port: 10200 zone_id: "default" } chunks { id: 1 data_ip: 1665335306 data_port: 10201 rpc_ip: 1665335306 rpc_port: 10200 zone_id: "default" } prioritized: false
W1108 13:08:19.076691 11910 zbs_client.cc:996] [Add TO RETRY QUEUE]: [IOCTX]: VEXTENT_WRITE seq: 180301206, volume: "fc0c28cf-bb15-492f-853c-c124e7147e6c", in_progress: 1, volume_offset: 80784592896, len: 4096, return_not_alloc: 0, resize: 0, retry_times: 0, next_retry_ms: 12409044778 , vextent no: 300, data_len: 4096, extent_offset: 63639552, pid: 0, owner_ip: "", owner_port: 0  queue size: 1 st:

[smartx@elf-172-20-128-83 16:33:18 ~]$ sudo zbs-iscsi lun show zbs-iscsi-datastore-1659425721511e 109
------------------  --------------------------------------------------------------------------------------------------
LUN Id              109
LUN Name            7789641e-8a77-4e0f-8be1-d884922127ec
Size                800.00 GiB
Unique Size         1.26 TiB
Shared Size         41.50 GiB
Volume id           01576676-f8a0-40f2-88c0-434d05cddc8d
Replica Num         2
Thin Provision      True
Creation Time       2022-08-22 17:09:23.728424456
Description
NAA                 3304d0a8fd6766751
Allowed Initiators  iqn.2013-11.org.smartx:578caceb-8cc2-42b9-9b4e-351dce0bfc2c.513f7ae2-d4e5-11ec-9890-b4055d3205f8.0
Single Access       True
PR                  Details
Prefer CID          0
Read Only           False
Secondary ID
Is Prioritized      False
Downgraded PRS      0.00 B
------------------  --------------------------------------------------------------------------------------------------
[smartx@elf-172-20-128-83 16:33:31 ~]$ sudo zbs-meta volume show_by_id 01576676-f8a0-40f2-88c0-434d05cddc8d --help
usage: zbs-meta volume show_by_id [-h] [--show_pextents] [--show_replica_distribution] volume_id

positional arguments:
  volume_id             volume id

options:
  -h, --help            show this help message and exit
  --show_pextents       show pextents
  --show_replica_distribution
                        show replica distribution
[smartx@elf-172-20-128-83 16:33:53 ~]$ sudo zbs-meta volume show_by_id 01576676-f8a0-40f2-88c0-434d05cddc8d --show_replica_distribution
--------------  ------------------------------------
ID              01576676-f8a0-40f2-88c0-434d05cddc8d
Name            01576676-f8a0-40f2-88c0-434d05cddc8d
Parent ID       92fe68e9-a01d-454e-bd78-e15c5ce4d9f0
Size            800.00 GiB
Unique Size     1.26 TiB
Shared Size     41.50 GiB
Creation Time   2022-08-22 17:09:23.728424456
Status          VOLUME_ONLINE
Replica Number  2
Thin Provision  True
Read Only       False
Description
Alloc Even      False
Clone count     2
Iops            0
Iops Read       0
Iops Write      0
Bps             0
Bps Read        0
Bps Write       0
Stripe Num      1
Stripe Size     262144
Prefer CID      0
Is Prioritized  False
Downgraded PRS  0.00 B
--------------  ------------------------------------
  chunk_id    total_replica_count    alive_replica_count
----------  ---------------------  ---------------------
         1                   3195                   3195
         3                   3172                   3172
         5                     33                     33


[smartx@elf-172-20-128-83 16:34:04 ~]$ sudo zbs-meta chunk list
  ID  IP          Host Name            Port  Storage Pool    Use State    Link Status        Data Capacity    Used Space    Provisioned Space    Allocated Space    Failure Data Space    Failure Cache Space    Cache Capacity    Used Cache Space    Planned PRS    Allocated PRS    Downgraded PRS    Registered Date      LSM Version    Zone     Maintenance Mode
----  ----------  -----------------  ------  --------------  -----------  -----------------  ---------------  ------------  -------------------  -----------------  --------------------  ---------------------  ----------------  ------------------  -------------  ---------------  ----------------  -------------------  -------------  -------  ------------------
   1  10.0.67.99  elf-172-20-128-83   10200  system          IN_USE       CONNECTED_HEALTHY  31.12 TiB        23.19 TiB     23.37 TiB            10.01 TiB          0.00 B                0.00 B                 0.00 B            0.00 B              0.00 B         0.00 B           0.00 B            2022-05-17 11:32:10  2.3.0          default  False
   3  10.0.67.87  elf-172-20-128-81   10200  system          IN_USE       CONNECTED_HEALTHY  25.84 TiB        21.25 TiB     21.43 TiB            9.14 TiB           0.00 B                0.00 B                 0.00 B            0.00 B              0.00 B         0.00 B           0.00 B            2022-05-17 11:32:20  2.3.0          default  False
   5  10.0.67.88  elf-172-20-128-82   10200  system          IN_USE       CONNECTED_HEALTHY  25.84 TiB        14.75 TiB     14.90 TiB            5.93 TiB           0.00 B                0.00 B                 0.00 B            0.00 B              0.00 B         0.00 B           0.00 B            2023-08-02 20:26:23  2.3.0          default  False



I1108 12:43:47.455657 14255 meta_rpc_server.cc:5707] [Get vhost io permission]: [REQUEST]: volume_id: "a2fc3e5f-7f0a-44bf-b8c0-c993ab5cb5bb" vm_uuid: "" machine_uuid: "", [RESPONSE]: ST:
Traceback:
[ENotLeader]:
, , [TIME]: 13 us.
I1108 12:43:47.459589 14255 meta_rpc_server.cc:5707] [Get vhost io permission]: [REQUEST]: volume_id: "9809b49a-20f7-4adf-9de5-f3c324a0de4e" vm_uuid: "" machine_uuid: "", [RESPONSE]: ST:
Traceback:
[ENotLeader]:
, , [TIME]: 10 us.
I1108 12:43:47.459869 14255 meta_rpc_server.cc:5707] [Get vhost io permission]: [REQUEST]: volume_id: "ffeb3374-5176-4e4c-97ec-1ca3666dc197" vm_uuid: "" machine_uuid: "", [RESPONSE]: ST:
Traceback:
[ENotLeader]:



 grep "ROLE CHANGE" /var/log/zbs/zbs-metad.log.20231106-102148.14245 
I1106 10:21:48.492027 14258 quorum_cluster.cc:466] [ROLE CHANGE] I am the leader now.  seq is 1295
I1108 12:43:49.137682 15567 quorum_cluster.cc:466] [ROLE CHANGE] I am the leader now.  seq is 1300
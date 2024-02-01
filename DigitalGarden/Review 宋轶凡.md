[CloudTower](https://tower.smartx.com/#/clusters/cl2emkioew67f0758exnnw152?view=%22vms%22&sidebar=%7B%22type%22%3A%22vm%22%2C%22view%22%3A%22detail%22%2C%22id%22%3A%22clqbq1ipwitbf09585912ho9l%22%7D) 比如这个虚拟机
虚拟机 Id 是 b847a0df-f3ed-4db7-9a5a-052965cb7e4d
进入目标集群的一个节点， 运行 如下命令
[smartx@dogfood-idc-elf-19-21-NVME-M2 15:30:53 ~]$ sudo zbs-tool elf show_vol_by_vm_id b847a0df-f3ed-4db7-9a5a-052965cb7e4d
Elf Volume                            Volume ID                             Name                                  Parent ID                             Size        Unique Size    Shared Size    Creation Time                  Status           Replica Number  Thin Provision    Read Only    Description    Alloc Even      Clone count    Prefer CID
------------------------------------  ------------------------------------  ------------------------------------  ------------------------------------  ----------  -------------  -------------  -----------------------------  -------------  ----------------  ----------------  -----------  -------------  ------------  -------------  ------------
0c2ef074-150e-4270-9da1-793b56e6ba45  f49281e2-6338-4318-9615-3599c3daf195  f49281e2-6338-4318-9615-3599c3daf195  d563d627-10f0-4c73-ab94-3af54aa3b4df  240.00 GiB  5.65 GiB       0.00 B         2023-12-19 10:24:55.122272698  VOLUME_ONLINE                 2  True              False                       False                     0             **0**


可以看到这个 虚拟机有一个磁盘，其 ELF Volume ID 为0c2ef074-150e-4270-9da1-793b56e6ba45， ZBS Volume ID 为  f49281e2-6338-4318-9615-3599c3daf195 
然后就可以通过这个 Volume ID 来查询副本分布情况了
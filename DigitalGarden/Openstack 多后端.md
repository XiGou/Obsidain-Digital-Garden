[Site Unreachable](https://medium.com/@ahakimx/deploy-openstack-all-in-one-with-kolla-ansible-f4bcfeb6ac57)
[Quick Start for deployment/evaluation — kolla-ansible 17.1.0.dev80 documentation](https://docs.openstack.org/kolla-ansible/latest/user/quickstart.html)
[Quick Start — kolla-ansible 15.3.1.dev24 documentation](https://docs.openstack.org/project-deploy-guide/kolla-ansible/zed/quickstart.html)
[Quickstart: AIO — openstack-ansible 26.4.1.dev1 documentation](https://docs.openstack.org/openstack-ansible/zed/user/aio/quickstart.html)
Centos Stream 9 Repo:[centos-stream | 镜像站使用帮助 | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/help/centos-stream/)

[Packstack — RDO](https://www.rdoproject.org/install/packstack/)

 setenforce 0

```
packstack --allinone --provision-image-url=http://192.168.17.20/home/wenquan/stash/cirros/0.3.4/cirros-0.3.4-x86_64-disk.img --os-swift-install=n --os-ceilometer-install=n
```

要将 CentOS ISO 挂载为本地软件包仓库（repo），可以按照以下步骤进行操作：

1. 创建一个目录来挂载 ISO 文件：

    ````
    sudo mkdir /mnt/iso
    ```
    
    这将在 `/mnt/iso` 目录下创建一个用于挂载 ISO 的目录。
    
    ````

2. 挂载 CentOS ISO 文件到指定目录：

    ````
    sudo mount -o loop /path/to/centos.iso /mnt/iso
    ```
    
    将 `/path/to/centos.iso` 替换为实际的 CentOS ISO 文件路径。这将将 ISO 文件挂载到 `/mnt/iso` 目录下。
    
    ````

3. 创建一个新的 repo 配置文件：

    `````
    sudo vi /etc/yum.repos.d/centos-iso.repo
    ```
    
    在编辑器中，添加以下内容：
    
    ````shell
    [centos-iso]
    name=CentOS ISO
    baseurl=file:///mnt/iso
    enabled=1
    gpgcheck=0
    ```
    
    保存并关闭文件。
    
    `````

4. 清除并重建 yum 缓存：

    ````
    sudo yum clean all
    sudo yum makecache
    ```
    
    这将清除旧的 yum 缓存并重新构建缓存，使其包含新挂载的 ISO 文件中的软件包信息。
    ````

现在，你已经将 CentOS ISO 成功挂载为本地软件包仓库（repo）。你可以使用 yum 命令来安装、更新或删除软件包，它将从挂载的 ISO 文件中获取软件包。请确保 CentOS ISO 文件的路径和名称与上述步骤中提供的路径一致。

[Site Unreachable](https://github.com/mustafaarif/packstack-openstack)

sudo modprobe nvme
sudo modprobe nvme-tcp
sudo modprobe nvme-fabrics

[openstack 管理二十二 - cinder 连接多个存储 backend-阿里云开发者社区](https://developer.aliyun.com/article/70813)

192.168.17.72

 cinder service-list
+------------------+-----------------------------------+------+---------+-------+----------------------------+---------+-----------------+---------------+
| Binary           | Host                              | Zone | Status  | State | Updated_at                 | Cluster | Disabled Reason | Backend State |
+------------------+-----------------------------------+------+---------+-------+----------------------------+---------+-----------------+---------------+
| cinder-scheduler | localhost.localdomain             | nova | enabled | up    | 2024-01-20T18:05:21.000000 | -       | -               |               |
| cinder-volume    | localhost.localdomain@lvm         | nova | enabled | down  | 2024-01-20T18:00:53.000000 | -       | -               | -             |
| cinder-volume    | localhost.localdomain@zbs-iscsi   | nova | enabled | up    | 2024-01-20T18:05:26.000000 | -       | -               | -             |
| cinder-volume    | localhost.localdomain@zbs-iscsi-2 | nova | enabled | up    | 2024-01-20T18:05:26.000000 | -       | -               | up            |
+------------------+-----------------------------------+------+---------+-------+----------------------------+---------+-----------------+---------------+

 File "/usr/lib/python3.9/site-packages/zbs/lib/utils.py", line 84, in <module>
2024-01-20 14:22:39.778 168159 ERROR cinder.cmd.volume     from google.protobuf.pyext._message import RepeatedCompositeContainer, Message

from google.protobuf.internal.containers import RepeatedCompositeFieldContainer

RepeatedCompositeContainer
openstack volume type create <type-name>

2024-01-20 14:36:54.652 169177 INFO cinder.volume.drivers.zbs.zbsiscsi [None req-b0d616b8-4ade-4721-82ed-fa67d7e246ae - - - - - -] _update_volume_status, {'volume_backend_name': 'zbs-iscsi', 'vendor_name': 'SmartX', 'driver_version': '2.0.0', 'storage_protocol': 'iSCSI', 'total_capacity_gb': 448813.0859375, 'free_capacity_gb': -15874.4140625, 'reserved_percentage': 0, 'QoS_support': True, 'multiattach': True}

[掌握 cinder-scheduler 调度逻辑 - 每天5分钟玩转 OpenStack（48）-阿里云开发者社区](https://developer.aliyun.com/article/311613)

```bash
extra_specs = resource_type.get('extra_specs', [])
if not extra_specs:
    return True
```

       没有extra specs 全部符合，不会过滤

```python
class ProtoStrWrap:
    _msg = None

    def __init__(self, msg):
        self._msg = msg

    def __getattr__(self, item):
        # import pdb;pdb.set_trace();
        value = getattr(self._msg, item)  # 确保字段存在
        #if isinstance(value, list):
        #    return [ProtoStrWrap(obj) if isinstance(obj, Message) else obj for obj in value]
        #elif isinstance(value, Message):
        #    return ProtoStrWrap(value)

        value_desc = self._msg.DESCRIPTOR.fields_by_name.get(item)
        if value_desc is None:
            return value

        if value_desc.type == value_desc.TYPE_MESSAGE and value_desc.label != value_desc.LABEL_REPEATED:
            return ProtoStrWrap(value)
        if value_desc.type == value_desc.TYPE_MESSAGE and value_desc.label == value_desc.LABEL_REPEATED:
            return [ProtoStrWrap(obj) for obj in value]

        if value_desc.type == value_desc.TYPE_BYTES and value_desc.label != value_desc.LABEL_REPEATED:
            return value.decode('utf-8')

        return value

    def __setattr__(self, item, value):
        if item == "_msg":
            super().__setattr__(item, value)
            return

        value_desc = self._msg.DESCRIPTOR.fields_by_name.get(item)
        if value_desc is None:
            raise AttributeError(f"Attribute '{item}' does not exist in the message")

        if isinstance(value, ProtoStrWrap):
            value = value._msg

        if value_desc.type == value_desc.TYPE_BYTES and value_desc.label != value_desc.LABEL_REPEATED:
            setattr(self._msg, item, value.encode('utf-8'))
        else:
            setattr(self._msg, item, value)

    def __str__(self):
        return str(self._msg)
```

```python 
#test_wrap.py
from zbs.lib.utils import ProtoStrWrap
from zbs.proto import meta_pb2 as meta                      
sub = meta.NVMFDistSubsystem
sub.name = b"xxx"
p=meta.Pool
p.name=b"sdfs"
sub.pool=p
#print(ProtoStrWrap(res).name)
#print(ProtoStrWrap(res).pool.name)
rsp = meta.NVMFDistSubsystemsResponse
rsp.dist_subsystems = [sub,sub]
for s in ProtoStrWrap(rsp).dist_subsystems:
  print(s.name)
```

use_multipath_for_image_xfer
forced

```python
self.connector = connector.InitiatorConnector.factory(
initiator.NVME, utils.get_root_helper(),
use_multipath=CONF.libvirt.volume_use_multipath,
device_scan_attempts=CONF.libvirt.num_nvme_discover_tries)
```

这说明 libvirt 也需要这个东西

```bash
def get_volume_connector(self, instance):
	root_helper = utils.get_root_helper()
	return connector.get_connector_properties(
	root_helper, CONF.my_block_storage_ip,
	CONF.libvirt.volume_use_multipath,
	enforce_multipath=True,
	host=CONF.host)
```

```bash
[root@localhost ~(keystone_admin)]# rpm -e --test python3-protobuf python3-google-api-core python3-grpcio-status-1.41.1-6.el9s.noarch  python3-googleapis-common-protos-1.56.4-2.el9s.noarch python3-google-api-client-2:2.58.0-1.el9s.noarch  python3-cinder-common-1:23.0.0-1.el9s.noarch python3-cinder-1:23.0.0-1.el9s.noarch openstack-cinder-1:23.0.0-1.el9s.noarch
[root@localhost ~(keystone_admin)]# 
```

```
 openstack volume type list  --long
 openstack volume create --type zbs-nvme --size 1 volume1
 openstack volume create --type zbs-iscsi --size 1 volume2
 openstack server create --flavor m1.tiny --image cirros --volume volume2 my-instance
 openstack server add volume my-instance volume1
```

在 zbs-client-py 上面测试 Warp 方式

```bash
[smartx@xi-gou-smtxos-6-0-0X20240123102334X1 18:32:26 ~]$ /usr/local/venv/zbs-client-py/bin/python test.py 
xxx
xxx
```

测试完了，可以兼容

## 实机测试
### 关闭 SELinux
 setenforce 0
[root@localhost ~]# getenforce
Permissive

sudo modprobe nvme
sudo modprobe nvme-tcp
sudo modprobe nvme-fabrics


### 加载 nvme 内核模块

```bash
[root@localhost ~]# sudo modprobe nvme
sudo modprobe nvme-tcp
sudo modprobe nvme-fabrics
[root@localhost ~]# lsmod | grep nvm
nvme_tcp               49152  0
nvme_fabrics           40960  1 nvme_tcp
nvme                   65536  0
nvme_core             221184  3 nvme_tcp,nvme,nvme_fabrics
nvme_common            24576  1 nvme_core
t10_pi                 24576  1 nvme_core
```

ip addr add 10.0.18.233/24 dev ens5



```bash
[root@localhost ~]# ip addr add 10.0.18.233/24 dev ens5
[root@localhost ~]# ping 10.0.18.72
PING 10.0.18.72 (10.0.18.72) 56(84) bytes of data.
64 bytes from 10.0.18.72: icmp_seq=1 ttl=64 time=1.44 ms
64 bytes from 10.0.18.72: icmp_seq=2 ttl=64 time=2.10 ms
^C
--- 10.0.18.72 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 1.437/1.768/2.099/0.331 ms
```

```bash
 glance image-list
+--------------------------------------+--------+
| ID                                   | Name   |
+--------------------------------------+--------+
| 48346636-e4aa-44a5-afdd-581b53ecde3a | cirros |
+--------------------------------------+--------+
```

```bash
openstack quota set  --volumes 100  admin
```

```bash
# 关闭selinux 
setenforce 0 ; sed -i 's/=enforcing/=disabled/g' /etc/sysconfig/selinux
```

### 安装 driver

```bash
Please select install option, should be one or more of
1) cinder
2) glance
3) Ops scripts
Please type in install options, multiple options need to separated by comma: 1
Set cinder to be glance backend, [yes|no]: no
Please type in cinder volume identify method for iSCSI, ignored for non-iSCSI case, [ lun_secondary_id | lun_uuid ], default lun_secondary_id: 
Please select the openstack python verison, [2|3]:3
site_path: /usr/lib/python3.9/site-packages
create zbs config
Please type in zbs_config_name, default is zbs.conf: 
Please type in cluster_id, default is zbscluster: 
Please type in zbs_proxy_ip, default is : 10.0.18.72
Please type in zbs_io_port, default is 10201: 
Please type in iscsi_rpc_port, default is 10206: 
Please type in meta_status_rpc_port, default is 10207: 
choose access protocol from [ iSCSI | NVMF ]
Please type in access_protocol, default is iSCSI: NVMF
choose nvmf trans type from [ tcp | rdma ]
Please type in nvmf_trans_type, default is tcp: 
Please type in backend_name, default is zbs-nvmf: 
Please type in backend_section_name, default is zbsnvmf: 
Please type in volume_type_name, default is zbs-nvmf: 
Please type in replica_num, default is 2: 
```

```
Please select install option, should be one or more of
1) cinder
2) glance
3) Ops scripts
Please type in install options, multiple options need to separated by comma: 1
Set cinder to be glance backend, [yes|no]: no
Please type in cinder volume identify method for iSCSI, ignored for non-iSCSI case, [ lun_secondary_id | lun_uuid ], default lun_secondary_id: 
Please select the openstack python verison, [2|3]:3
site_path: /usr/lib/python3.9/site-packages
create zbs config
Please type in zbs_config_name, default is zbs.conf: zbs-iscsi.conf
Please type in cluster_id, default is zbscluster: 
Please type in zbs_proxy_ip, default is : 10.0.18.72
Please type in zbs_io_port, default is 10201: 
Please type in iscsi_rpc_port, default is 10206: 
Please type in meta_status_rpc_port, default is 10207: 
choose access protocol from [ iSCSI | NVMF ]
Please type in access_protocol, default is iSCSI: 
Please type in iscsi_target_portal_port, default is 3260: 
Please type in backend_name, default is zbs-iscsi: 
Please type in backend_section_name, default is zbsiscsi: 
Please type in volume_type_name, default is zbs-iscsi: 
Please type in replica_num, default is 2: 
```

### 建立type

```
openstack volume type create --property volume_backend_name='zbs-nvmf' nvmf
openstack volume type create --property volume_backend_name='zbs-iscsi' zbs-iscsi
```

[root@localhost zbs-openstack-driver(keystone_admin)]#  openstack volume type list  --long
+--------------------------------------+-------------+-----------+---------------------+--------------------------------+
| ID                                   | Name        | Is Public | Description         | Properties                     |
+--------------------------------------+-------------+-----------+---------------------+--------------------------------+
| 6b422b8e-4dec-418a-8565-1baadfe5966d | q           | True      |                     | volume_backend_name='zbs-nvmf'

my_block_storage_ip=存储ip

```bash
 Skipping ('traddr=10.0.18.76,trsvcid=8009,src_addr=10.0.18.234', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), not part of portals [('traddr=10.0.18.76,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.75,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.73,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.74,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892')] set_portals_controllers /usr/lib/python3.9/site-packages/os_brick/initiator/connectors/nvmeof.py:431
```

## BUG Bocat

```bash
# sysfs_property
DEBUG os_brick.initiator.connectors.nvmeof [None req-ba0f2b04-023e-4761-a959-24c63e76a0aa 428aa10a53ba4ceda4a0912f0e806b31 43fc55cbc983468c964c5946c4a3d1ad - - default default] Search controllers for portals [('traddr=10.0.18.76,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.75,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.73,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.74,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892')] set_portals_controllers /usr/lib/python3.9/site-packages/os_brick/initiator/connectors/nvmeof.py:397

# 
DEBUG os_brick.initiator.connectors.nvmeof [None req-6102f6af-753a-447e-83f3-2e05f25b1b16 428aa10a53ba4ceda4a0912f0e806b31 43fc55cbc983468c964c5946c4a3d1ad - - default default] Skipping ('traddr=10.0.18.73,trsvcid=8009,src_addr=10.0.18.234', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), not part of portals [('traddr=10.0.18.76,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.75,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.73,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892'), ('traddr=10.0.18.74,trsvcid=8009', 'tcp', 'nqn.2014-08.org.nvmexpress:uuid:8e1495c5-196b-48a1-bc47-25888e88c892')] set_portals_controllers /usr/lib/python3.9/site-packages/os_brick/initiator/connectors/nvmeof.py:431
```

后一个是从系统中获取的

```python
address = sysfs_property('address', ctrl_path)
# address: traddr=10.0.18.76,trsvcid=8009,src_addr=10.0.18.234
            if not address:
                LOG.error("Couldn't read address for %s", ctrl_path)
                continue
            ctrl_address = dict((x.split('=')
                                 for x in address.split(',')))

            ctrl_addr = ctrl_address['traddr']
            ctrl_port = ctrl_address['trsvcid']
```

 pip install os-brick==6.6.0
os-brick 的 nvme-of 仍在积极更新中，所以必须升级到最新
主要 commit 



```bash
  **[Gorka Eguileor](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-sandbox/workbench/workbench.html "Email Gorka Eguileor (geguileo@redhat.com)")**, 5 months ago   _(September 14th, 2023 4:38 PM)_ 

> NVMe-oF: Improve hostnqn creation  
>   
> This patch improves the creation of the /etc/nvme/hostnqn file by using  
> the system UUID value we usually already know.  
>   
> This saves us one or two calls to the nvme-cli command and it also  
> allows older nvme-cli versions that don't have the `show-hostnqn`  
> command or have it but can only read from file to generate the same  
> value every time, which may be useful when running inside a container  
> under some circumstances.  
>   
> Change-Id: Ib250d213295695390dbdbb3506cb297a86e95218  
> (cherry picked from commit 088c676bf5d1846efc19385433a65c1675c2ebd1)
```

```
openstack server create --flavor m1.tiny --image cirros  firstvm
nova volume-attach firstvm d4be93e7-8997-4938-b113-2922829fbb26
openstack volume create --image cirros --size 1 --type zbs-nvmf 
```

### detach 问题
https://docs.openstack.org/cinder/latest/configuration/block-storage/service-token.html

```bash
 detach_device /usr/lib/python3.9/site-packages/nova/virt/libvirt/guest.py:465
2024-01-31 20:51:05.949 24634 DEBUG nova.virt.libvirt.driver [None req-7bf70339-4fcb-4a91-9e86-1585e8980f3d - - - - - -] Received event <DeviceRemovedEvent: 1706752265.9481127, 189b5d39-f300-4e55-a74a-85153544df82 => virtio-disk7> from libvirt while the driver is waiting for it; dispatched. emit_event /usr/lib/python3.9/site-packages/nova/virt/libvirt/driver.py:2370
2024-01-31 20:51:05.950 24634 DEBUG oslo_service.periodic_task [None req-9c3186c8-dc6b-47ae-9039-39d8262f0679 - - - - - -] Running periodic task ComputeManager._cleanup_running_deleted_instances run_periodic_tasks /usr/lib/python3.9/site-packages/oslo_service/periodic_task.py:210
2024-01-31 20:51:05.953 24634 DEBUG nova.virt.libvirt.driver [None req-fa6f7b36-ad58-4c90-95d5-e0e7ea211774 428aa10a53ba4ceda4a0912f0e806b31 43fc55cbc983468c964c5946c4a3d1ad - - default default] Start waiting for the detach event from libvirt for device vdh with device alias virtio-disk7 for instance 189b5d39-f300-4e55-a74a-85153544df82 _detach_from_live_and_wait_for_event /usr/lib/python3.9/site-packages/nova/virt/libvirt/driver.py:2599
2024-01-31 20:51:05.956 24634 INFO nova.virt.libvirt.driver [None req-fa6f7b36-ad58-4c90-95d5-e0e7ea211774 428aa10a53ba4ceda4a0912f0e806b31 43fc55cbc983468c964c5946c4a3d1ad - - default default] Successfully detached device vdh from instance 189b5d39-f300-4e55-a74a-85153544df82 from the live domain config.
2024-01-31 20:51:05.963 24634 DEBUG nova.virt.libvirt.volume.nvme [None req-fa6f7b36-ad58-4c90-95d5-e0e7ea211774 428aa10a53ba4ceda4a0912f0e806b31 43fc55cbc983468c964c5946c4a3d1ad - - default default] [instance: 189b5d39-f300-4e55-a74a-85153544df82] Disconnecting NVMe disk disconnect_volume /usr/lib/python3.9/site-packages/nova/virt/libvirt/volume/nvme.py:50
2024-01-31 20:51:05.964 24634 DEBUG os_brick.initiator.connectors.nvmeof [None req-fa6f7b36-ad58-4c90-95d5-e0e7ea211774 428aa10a53ba4ceda4a0912f0e806b31 43fc55cbc983468c964c5946c4a3d1ad - - default default] ==> disconnect_volume: call "{'args': (<os_brick.initiator.connectors.nvmeof.NVMeOFConnector object at 0x7fb43c2116a0>, {'nqn': 'nqn.2020-12.com.smartx:system:cinder-3f89b89e-d958-11ed-af17-25e52532610e', 'transport_type': 'tcp', 'volume_nguid': 'cc538d96-393f-4179-b94d-7fc73fb89b01', 'discard': False, 'volume_replicas': [{'vol_uuid': 'cc538d96-393f-4179-b94d-7fc73fb89b01', 'target_nqn': 'nqn.2020-12.com.smartx:system:cinder-3f89b89e-d958-11ed-af17-25e52532610e', 'portals': [['10.0.18.76', 8009, 'tcp'], ['10.0.18.75', 8009, 'tcp'], ['10.0.18.73', 8009, 'tcp'], ['10.0.18.74', 8009, 'tcp'|'10.0.18.76', 8009, 'tcp'], ['10.0.18.75', 8009, 'tcp'], ['10.0.18.73', 8009, 'tcp'], ['10.0.18.74', 8009, 'tcp']]}], 'replica_count': 1, 'vol_uuid': '133ba1ae-264a-4398-9a4a-171339326aff', 'alias': 'volume-133ba1ae-264a-4398-9a4a-171339326aff', 'qos_specs': None, 'access_mode': 'rw', 'encrypted': False, 'cacheable': False, 'device_path': '/dev/nvme0n25'}, None), 'kwargs': {'force': False}}" trace_logging_wrapper /usr/lib/python3.9/site-packages/os_brick/utils.py:176
```

## 全新环境测试

Centos Stream 9 部署 Openstack 2023.2 Bobcat
对于 NVMF， os-brick 必须升级到 6.6.0 版本才可以使用，官方发布的 6.6.0 有 pip 包， 没找到 RPM， 需要自己构建。
Zed 版本安装在 CentOS 9， NVMF 将不可用
2023.1， 2023.2 均需要升级 os-brick 获得更新以后才能正常使用

```bash
cp -f /usr/local/lib/python3.9/site-packages/os_brick/utils.py /usr/lib/python3.9/site-packages/os_brick 
cp -f /usr/local/lib/python3.9/site-packages/os_brick/privileged/nvmeof.py /usr/lib/python3.9/site-packages/os_brick/privileged/
 cp -f /usr/local/lib/python3.9/site-packages/os_brick/initiator/connectors/nvmeof.py  /usr/lib/python3.9/site-packages/os_brick/initiator/connectors/
```
### 部署 Driver
```
[root@localhost ~]# tar -xzvf zbs-openstack-driver.tgz 
zbs-openstack-driver/crudini
zbs-openstack-driver/zbs.tar
zbs-openstack-driver/proto2.tar
zbs-openstack-driver/proto3.tar
zbs-openstack-driver/cinder.tar
zbs-openstack-driver/glance.tar
zbs-openstack-driver/zbsops.tar
zbs-openstack-driver/install.sh
zbs-openstack-driver/iniparse.tar
zbs-openstack-driver/os-brick.filters
zbs-openstack-driver/os_brick/
zbs-openstack-driver/os_brick/utils.py
zbs-openstack-driver/os_brick/nvme.py
zbs-openstack-driver/os_brick/connector.py
zbs-openstack-driver/nvmeof_agent/
zbs-openstack-driver/nvmeof_agent/nvmeof_agent.py
zbs-openstack-driver/nvmeof_agent/nvmeof_agent.service
[root@localhost ~]# cd zbs-openstack-driver/
[root@localhost zbs-openstack-driver]# ./install.sh 
iniparse/
iniparse/compat.py
iniparse/configparser.py
iniparse/__init__.py
iniparse/config.py
iniparse/utils.py
iniparse/ini.py
./install.sh: line 42: [: =: unary operator expected
./install.sh: line 56: [: =: unary operator expected
Please select install option, should be one or more of
1) cinder
2) glance
3) Ops scripts
Please type in install options, multiple options need to separated by comma: 1
Set cinder to be glance backend, [yes|no]: no
Please type in cinder volume identify method for iSCSI, ignored for non-iSCSI case, [ lun_secondary_id | lun_uuid ], default lun_secondary_id: 
Please select the openstack python verison, [2|3]:3
site_path: /usr/lib/python3.9/site-packages
create zbs config
Please type in zbs_config_name, default is zbs.conf: zbs-iscsi.conf
Please type in cluster_id, default is zbscluster: 
Please type in zbs_proxy_ip, default is : 10.0.18.72
Please type in zbs_io_port, default is 10201: 
Please type in iscsi_rpc_port, default is 10206: 
Please type in meta_status_rpc_port, default is 10207: 
choose access protocol from [ iSCSI | NVMF ]
Please type in access_protocol, default is iSCSI: 
Please type in iscsi_target_portal_port, default is 3260: 
Please type in backend_name, default is zbs-iscsi: 
Please type in backend_section_name, default is zbsiscsi: 
Please type in volume_type_name, default is zbs-iscsi: 
Please type in replica_num, default is 2: 
```

```bash
Please select install option, should be one or more of
1) cinder
2) glance
3) Ops scripts
Please type in install options, multiple options need to separated by comma: 1
Set cinder to be glance backend, [yes|no]: no
Please type in cinder volume identify method for iSCSI, ignored for non-iSCSI case, [ lun_secondary_id | lun_uuid ], default lun_secondary_id: 
Please select the openstack python verison, [2|3]:3
site_path: /usr/lib/python3.9/site-packages
create zbs config
Please type in zbs_config_name, default is zbs.conf: zbs-nvmf.conf
Please type in cluster_id, default is zbscluster: 
Please type in zbs_proxy_ip, default is : 10.0.18.72
Please type in zbs_io_port, default is 10201: 
Please type in iscsi_rpc_port, default is 10206: 
Please type in meta_status_rpc_port, default is 10207: 
choose access protocol from [ iSCSI | NVMF ]
Please type in access_protocol, default is iSCSI: NVMF
choose nvmf trans type from [ tcp | rdma ]
Please type in nvmf_trans_type, default is tcp: 
Please type in backend_name, default is zbs-nvmf: 
Please type in backend_section_name, default is zbsnvmf: 
Please type in volume_type_name, default is zbs-nvmf: 
Please type in replica_num, default is 2: 
```
### 重启服务
[root@localhost zbs-openstack-driver]# systemctl restart openstack-cinder*
[root@localhost zbs-openstack-driver]# systemctl restart openstack-nova*



### 建立 Type
```
[root@localhost ~(keystone_admin)]# openstack volume type create --property volume_backend_name='zbs-nvmf' nvmf
+-------------+--------------------------------------+
| Field       | Value                                |
+-------------+--------------------------------------+
| description | None                                 |
| id          | b4a56c0f-17ef-4763-86bd-d9cc81a682bd |
| is_public   | True                                 |
| name        | nvmf                                 |
| properties  | volume_backend_name='zbs-nvmf'       |
+-------------+--------------------------------------+
[root@localhost ~(keystone_admin)]# openstack volume type create --property volume_backend_name='zbs-iscsi' zbs-iscsi
+-------------+--------------------------------------+
| Field       | Value                                |
+-------------+--------------------------------------+
| description | None                                 |
| id          | b9b9ee02-79ba-421e-860b-727446d6a43d |
| is_public   | True                                 |
| name        | zbs-iscsi                            |
| properties  | volume_backend_name='zbs-iscsi'      |
+-------------+--------------------------------------+
```
### 创建两种后端的 Volume
```bash
openstack volume create --type nvmf --size 1 volume1
openstack volume create --type zbs-iscsi --size 1 volume1
[root@localhost ~(keystone_admin)]#  openstack volume list
+--------------------------------------+---------+-----------+------+-------------+
| ID                                   | Name    | Status    | Size | Attached to |
+--------------------------------------+---------+-----------+------+-------------+
| b511ff58-cde6-4943-a283-af415ff14432 | volume1 | available |    1 |             |
| 5dfccb91-4383-47ff-b6cd-c98e205aad3c | volume1 | available |    1 |             |
+--------------------------------------+---------+-----------+------+-------------+
```

### 扩容
```
[root@localhost ~(keystone_admin)]# openstack volume set --size 2 b511ff58-cde6-4943-a283-af415ff14432 
[root@localhost ~(keystone_admin)]# openstack volume set --size 2 5dfccb91-4383-47ff-b6cd-c98e205aad3c
[root@localhost ~(keystone_admin)]#  openstack volume list
+--------------------------------------+---------+-----------+------+-------------+
| ID                                   | Name    | Status    | Size | Attached to |
+--------------------------------------+---------+-----------+------+-------------+
| b511ff58-cde6-4943-a283-af415ff14432 | volume1 | available |    2 |             |
| 5dfccb91-4383-47ff-b6cd-c98e205aad3c | volume1 | available |    2 |             |
+--------------------------------------+---------+-----------+------+-------------+
```

[root@localhost ~(keystone_admin)]# lsmod | grep nvm
nvme_tcp               49152  0
nvme_fabrics           40960  1 nvme_tcp
nvme                   65536  0
nvme_core             221184  3 nvme_tcp,nvme,nvme_fabrics
nvme_common            24576  1 nvme_core
t10_pi                 24576  2 sd_mod,nvme_core
### 从 image 建立 Volume
openstack volume create --image cirros --size 1 --type nvmf 
openstack volume create --image cirros --size 1 --type zbs-iscsi 
### 启动 mutipathd
[root@localhost ~(keystone_admin)]# systemctl status multipathd
● multipathd.service - Device-Mapper Multipath Device Controller
     Loaded: loaded (/usr/lib/systemd/system/multipathd.service; enabled; preset: enabled)
     Active: active (running)

### iSCSI 协议 Attach
nova volume-attach firstvm b511ff58-cde6-4943-a283-af415ff14432
[root@localhost ~(keystone_admin)]# cinder show  b511ff58-cde6-4943-a283-af415ff14432
+--------------------------------+------------------------------------------+
| Property                       | Value                                    |
+--------------------------------+------------------------------------------+
| attached_servers               | ['189b5d39-f300-4e55-a74a-85153544df82'] |
| attachment_ids                 | ['a8fb18f8-2d63-422b-a865-9378b5bbcd68'] |
| volume_type                    | zbs-iscsi                                |
| volume_type_id                 | b9b9ee02-79ba-421e-860b-727446d6a43d     |

### NVMF 协议 Attach
[root@localhost ~(keystone_admin)]# nova volume-attach firstvm d9a2b5cd-6797-4881-805a-117d8e8c0829^C
[root@localhost ~(keystone_admin)]# cinder show d9a2b5cd-6797-4881-805a-117d8e8c0829
+--------------------------------+------------------------------------------+
| Property                       | Value                                    |
+--------------------------------+------------------------------------------+
| attached_servers               | ['189b5d39-f300-4e55-a74a-85153544df82'] |
| attachment_ids                 | ['e763da5e-c2da-44b1-9569-bbb60b57770f'] |
| volume_type                    | nvmf                                     |
| volume_type_id                 | b4a56c0f-17ef-4763-86bd-d9cc81a682bd     |
+--------------------------------+------------------------------------------+


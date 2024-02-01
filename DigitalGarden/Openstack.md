# Openstack

Centos 7.3 1611

```bash
[kilo]
name=el73-kilo
baseurl=https://mirrors.bfsu.edu.cn/centos-vault/7.3.1611/cloud/x86_64/openstack-kilo/
gpgcheck=0
enabled=1

[virt]
name=kvm-common
baseurl=https://mirrors.bfsu.edu.cn/centos-vault/7.3.1611/virt/x86_64/kvm-common/
gpgcheck=0
enabled=1
```

```bash
systemctl stop NetworkManager.service
systemctl disable NetworkManager.service
systemctl stop firewalld
systemctl disable firewalld

# 关闭selinux 
setenforce 0 ; sed -i 's/=enforcing/=disabled/g' /etc/sysconfig/selinux

yum install -y qemu-kvm libvirt-client libvirt-daemon libvirt-daemon-driver-qemu python-setuptools
yum install -y libvirt-client libvirt-daemon libvirt-daemon-driver-qemu 
```

```bash
yum install openstack-packstack -y
```

```

packstack --gen-answer-file=gouxi-misaka.txt

packstack --answer-file=gouxi-misaka.txt

@@ -38,18 +38,18 @@ CONFIG_NEUTRON_INSTALL=y
 CONFIG_HORIZON_INSTALL=y

 # Specify 'y' to install OpenStack Object Storage (swift). ['y', 'n']
-CONFIG_SWIFT_INSTALL=y
+CONFIG_SWIFT_INSTALL=n

 # Specify 'y' to install OpenStack Metering (ceilometer). ['y', 'n']
-CONFIG_CEILOMETER_INSTALL=y
+CONFIG_CEILOMETER_INSTALL=n

 # Specify 'y' to install OpenStack Telemetry Alarming (Aodh). Note
 # Aodh requires Ceilometer to be installed as well. ['y', 'n']
-CONFIG_AODH_INSTALL=y
+CONFIG_AODH_INSTALL=n

 # Specify 'y' to install OpenStack Metering as a Service (gnocchi).
 # ['y', 'n']
-CONFIG_GNOCCHI_INSTALL=y
+CONFIG_GNOCCHI_INSTALL=n

 # Specify 'y' to install OpenStack Events Service (panko). ['y', 'n']
 CONFIG_PANKO_INSTALL=n
@@ -101,11 +101,11 @@ CONFIG_DEBUG_MODE=n
 CONFIG_CONTROLLER_HOST=192.168.93.21

 # List the servers on which to install the Compute service.
-CONFIG_COMPUTE_HOSTS=192.168.93.21
+CONFIG_COMPUTE_HOSTS=192.168.93.21,192.168.93.22
```

```bash
# mikata
packstack --install-hosts=192.168.31.71,192.168.30.62 --os-compute-hosts=192.168.31.71,192.168.30.62 --provision-image-url=http://192.168.25.178:8081/images/cirros-0.3.4-x86_64-disk.img --os-swift-install=n --os-ceilometer-install=n --os-aodh-install=n --os-gnocchi-install=n 
```

```bash
# kilo
packstack --allinone --provision-image-url=http://192.168.25.178:8081/images/cirros-0.3.4-x86_64-disk.img --os-swift-install=n --os-ceilometer-install=n
```

```bash
. keystonerc_admin
openstack volume create gx --size 10
openstack volume show gx
```

```bash
wget http://download.cirros-cloud.net/0.3.4/cirros-0.3.4-x86_64-disk.img
wget http://192.168.25.178:8081/images/cirros-0.3.4-x86_64-disk.img
openstack image create cirros --file cirros-0.3.4-x86_64-disk.img
glance image-create --name cirros --disk-format qcow2 --container-format=bare  --file cirros-0.3.4-x86_64-disk.img

nova list
glance image-list
nova flavor-list
nova boot --image cirros --flavor m1.tiny firstvm
nova list
nova service-list

openstack flavor create flv1 --ram 256 --disk 3

nova boot --flavor b1411bba-5abe-4ade-862e-d14d3e2adf95 --block-device id=893b4918-22b6-4f01-a4b6-2d3ffa51cc79,source=image,dest=volume,size=50,bootindex=0 --nic net-id=54109dbc-efd4-4709-b379-5e9e2c282b97 --availability-zone az01.cell01.test.cn test1
nova  get-vnc-console test novnc
openstack flavor create flv1 --ram 256 --disk 3
nova boot --flavor flv1 --block-device id=893b4918-22b6-4f01-a4b6-2d3ffa51cc79,source=image,dest=volume,size=1,bootindex=0 test1
```

[Error Message "Virtual Interface creation failed" Is Displayed During VM Creation_Kunpeng BoostKit for Virtualization_Troubleshooting_OpenStack Stein Troubleshooting Cases_HUAWEI CLOUD](https://support.huaweicloud.com/intl/en-us/trouble-kunpengcpfs/kunpengopenstackstein_09_0010kunpengopenstackstein_09_0021.html)

[Error "internal error cannot find character device" when trying to connect a domain's console](https://wiki.libvirt.org/page/Error_%22internal_error_cannot_find_character_device%22_when_trying_to_connect_a_domain%27s_console)

[Bug #1340518 "can not use virsh console on serial terminal" : Bugs : OpenStack Compute (nova)](https://bugs.launchpad.net/nova/+bug/1340518)

带命令行安装

[How to install OpenStack Mitaka in 20 minutes using RDO [video]](https://fleio.com/blog/2016/05/20/how-to-install-openstack-mitaka-using-rdo-video/)

> Just in case, it's possible with Juno to get the virsh console as a telnet endpoint.
> 
> 
> I have achieved this adding the following config to /etc/nova/nova.conf
> 
> [serial_console]enabled = True
> 
> It will create all the serial ports as telnet ports in 10000 onwards. Have the config here (for Juno):
> 
> [https://github.com/openstack/nova/blob/c5ac21f3dbb4ad59efcb631d91e4e64f77fba43f/nova/console/serial.py#L33-L52](https://github.com/openstack/nova/blob/c5ac21f3dbb4ad59efcb631d91e4e64f77fba43f/nova/console/serial.py#L33-L52)
> 
> **`systemctl restart openstack-nova-*`**
> 

[](https://oyd.mawan.info/2019/05/03/multi-node-openstack-installation-steps-on-centos-7/)

[Installing OpenStack "Kilo" release on CentOS 7](https://megrezblogs.wordpress.com/2016/11/21/installing-openstack-kilo-release-on-centos-7/)

# ZBS Cluster

## add cinder driver

```python
a)     modify cinder conf
[zbs]
volume_driver=cinder.volume.drivers.zbs.ZBSDriver
rbd_max_clone_depth=5
glance_api_version=2
zbs_meta_ip=10.54.28.32

change the backend to zbs.

systemctl restart openstack-cinder-volume

b)     add Cinder Driver
/usr/lib/python2.7/site-packages/cinder/volume/drivers/zbs.py
add the content of this driver.
```

```bash
systemctl restart openstack-cinder-volume
cinder create 1
```

[探索 OpenStack 之（9）：深入块存储服务Cinder （功能篇）-阿里云开发者社区](https://developer.aliyun.com/article/378435)

## add glance config

[Glance/Cinder-store-proposal](https://wiki.openstack.org/wiki/Glance/Cinder-store-proposal)

```bash
vi /etc/glance/glance-api.conf

change:
stores=glance.store.filesystem.Store, glance.store.http.Store, glance.store.cinder.Store
default_store='cinder'

systemctl restart openstack-glance-api
```

```bash
glance --debug image-create --name cirros2 --file cirros-0.3.4-x86_64-disk.img --disk-format raw --container-format bare
```

[https://docs.google.com/document/d/1CQTdIgIV9GhQkisFLE2XL283PytYs1u0SInSPEwYSI0/edit#](https://docs.google.com/document/d/1CQTdIgIV9GhQkisFLE2XL283PytYs1u0SInSPEwYSI0/edit#)

## 升级版本

建立venv

```bash
docker run --rm -it  -v $PWD:/root/zbs registry.smtx.io/gouxi/zbs-build bash

systemctl stop zbs-metad zbs-chunkd
systemctl start zbs-metad zbs-chunkd
systemctl status zbs-metad zbs-chunkd

dd if=/dev/urandom of=test.file count=1024 bs=1K
for i in {1..1024}; do cat test.file  >> test1.file; done

zbs-chunk disk write default 1 -i test.file
zbs-chunk disk read default 1 -o /dev/null

zbs-chunk volume write default 1 -i test.file
zbs-chunk volume read default 1 -o /dev/null

zbs-chunk journal list
zbs-chunk journal umount /dev/sda3
zbs-chunk journal format /dev/sda3 --force
zbs-chunk journal mount /dev/sda3

yum  search zbs --show-duplicate | grep zbs-3

yum install zbs-3.0.22-rc4.0.release.git.g541c09ad0.el7.SMTX.x86_64 libzbs-3.0.22-rc4.0.release.git.g541c09ad0.el7.SMTX.x86_64
yum install zbs-proto-3.0.22-rc4.0.release.git.g541c09ad0.el7.SMTX.x86_64
yum install zbs-debuginfo-3.0.22-rc4.0.release.git.g541c09ad0.el7.SMTX.x86_64

systemctl daemon-reload

tail -f /var/log/zbs/zbs-metad.INFO
tail -f /var/log/zbs/zbs-chunkd.INFO

yum install python2-pip
yum install python2-virtualenv

wget http://192.168.17.20/repo/pub/smartxos/el7/2-qa/x86_64/pyzbs-3.0.22-rc3.0.git.g73e327929.el7.centos.x86_64.rpm
rpm -Uvh pyzbs-3.0.22-rc3.0.git.g73e327929.el7.centos.x86_64.rpm --nodeps

yum install zbs-4.0.14-rc2.0.release.git.gc9817929c.el7.SMTX.HCI.x86_64 libzbs-4.0.14-rc2.0.release.git.gc9817929c.el7.SMTX.HCI.x86_64
yum install zbs-proto-4.0.14-rc2.0.release.git.gc9817929c.el7.SMTX.HCI.x86_64
yum install zbs-debuginfo-4.0.14-rc2.0.release.git.gc9817929c.el7.SMTX.HCI.x86_64
wget http://192.168.17.20/repo/pub/smartxos/el7/2-rc/x86_64/pyzbs-4.0.14-rc2.0.git.g5ed520cbd.el7.centos.x86_64.rpm
rpm -Uvh pyzbs-4.0.14-rc2.0.git.g5ed520cbd.el7.centos.x86_64.rpm --nodeps
systemctl daemon-reload
systemctl start zbs-metad zbs-chunkd zbs-taskd zbs-iscsi-redirectord
systemctl status  zbs-metad zbs-chunkd zbs-taskd zbs-iscsi-redirectord

wget http://192.168.17.20/gouxi/zbs20xupgrade/update_superblock.py && chmod +x update_superblock.py

zbs-meta volume create default cinder-dfasdfasdfasdfsdfdfasdf 1

要记得enable service

/etc/sysconfig/zbs-chunkd ACCESS_ENABLE_ISCSI=true

rsync -av /usr/lib/python2.7/site-packages/nova  /usr/lib/python2.7/site-packages/cinder /usr/lib/python2.7/site-packages/cinderclient root@192.168.23.23:/usr/lib/python2.7/site-packages/
rsync -av  /usr/lib/python2.7/site-packages/cinderclient root@192.168.23.23:/usr/lib/python2.7/site-packages/
```

```bash
wget -r -np -nH -nd http://192.168.17.20/gouxi/zbs20xupgrade/
chmod +x *.py
pip3 install crc32c leveldb protobuf==3.19.6
```

# install zbs20x

79  yum makecache
80  yum search --show_duplicates zbs | grep 2.1.4
81  yum search --show-duplicates zbs | grep 2.1.4
82  yum install zbs-2.1.4-rc2.0.git.g29c91df66.el7.centos.x86_64
83  vi /etc/yum.repos.d/smartx.repo
84  yum install zbs-2.1.4-rc2.0.git.g29c91df66.el7.centos.x86_64
85  vi /etc/yum.conf
86  yum install v8-3.14.5.10-14.el7.x86_64
87  yum install zbs-2.1.4-rc2.0.git.g29c91df66.el7.centos.x86_64
88  yum search --show-duplicates zbs | grep 2.1.4
89  yum install libzbs-2.1.4-rc2.0.git.g29c91df66.el7.centos.x86_64
90  yum install
91  yum search --show-duplicates zbs | grep 2.1.4
92  yum search --show-duplicates pyzbs | grep 2.
93  wget [http://192.168.17.20/repo/pub/smartxos/el7/2-qa/x86_64/pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm](http://192.168.17.20/repo/pub/smartxos/el7/2-qa/x86_64/pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm)
94  yum install pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
95  yum install python-flask-mongoengine
96  yum install python-cpuinfo
97  yum install [python-web.py](http://python-web.py/) python-prctl
98  yum install python-pysmart python-librsync python-flask-bcrypt python-flask-debugtoolbar
99  rpm -e --test    python-jinja2
100  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
101  yum install python-click
102  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
103  yum install python-daemon protobuf-python
104  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
105  yum install python-mongoengine python-librsync python-kazoo
106  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
107  yum install smartmontools [python-web.py](http://python-web.py/) python-snappy python-pysmart
108  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
109  yum install python-flask python-flask-bcrypt python-flask-debugtoolbar  python-flask-login python-flask-mongoengine
110  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
111  yum install nginx
112  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
113  yum install python-gunicorn python-gevent python-astroid python-prctl python-py-cpuinfo
114  yum makecache
115  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm
116  yum install python-py-cpuinfo python-prctl python-pysmart [python-web.py](http://python-web.py/)
117  rpm -ivh pyzbs-2.1.4-0.git.1db9bf49.el7.centos.x86_64.rpm --nodeps
118  zbs-meta -h
119  wget [http://192.168.17.20/repo/pub/kylin/ky10/smartx-iso-common/x86_64/Packages/python-prctl-1.6.1-1.ky10.x86_64.rpm](http://192.168.17.20/repo/pub/kylin/ky10/smartx-iso-common/x86_64/Packages/python-prctl-1.6.1-1.ky10.x86_64.rpm)
120  rpm -ivh python-prctl-1.6.1-1.ky10.x86_64.rpm
121  zbs-meta -h
122  yum install pylint
123  zbs-meta -h
124  yum search python-webpy
125  yum install python-webpy
126  zbs-meta -h
127  zbs-meta cluster summary
128  rpm -qa | grep zookee
129  ip a
130  vi /etc/sysconfig/network-scripts/ifcfg-eth1
131  ifup eth1
132  ip a
133  ci /etc/zbs/smartxos.cfg
134  vi /etc/zbs/smartxos.cfg
135  rpm -qa | grep nongo
136  rpm -qa | grep mongo
137  lsblk
138  vi /etc/zbs/smartxos.cfg
139  ip a
140  cd /usr/share/zbs/bin
141   ./zbs_firstboot.sh --zbsonly
142  ls
143  history

改时间破解license  date -s 09/03/2022

# 联通测试环境信息:

```bash
[root@RGCC01 ~]# rpm -qa |grep openstack
openstack-nova-objectstore-2015.1.0-20170106.el7.noarch
openstack-neutron-common-2015.1.0-20161230.el7.noarch
openstack-neutron-lbaas-2015.1.0-20161230.el7.noarch
openstack-neutron-openvswitch-2015.1.0-20161230.el7.noarch
openstack-nova-scheduler-2015.1.0-20170106.el7.noarch
openstack-nova-2015.1.0-20170106.el7.noarch
openstack-trove-2016.1.0-20161230.el7.noarch
openstack-glance-2015.1.0-20161230.el7.noarch
openstack-trove-conductor-2016.1.0-20161230.el7.noarch
openstack-nova-api-2015.1.0-20170106.el7.noarch
openstack-nova-cert-2015.1.0-20170106.el7.noarch
openstack-nova-conductor-2015.1.0-20170106.el7.noarch
openstack-nova-compute-2015.1.0-20170106.el7.noarch
openstack-cinder-2015.1.0-20161230.el7.noarch
openstack-trove-common-2016.1.0-20161230.el7.noarch
openstack-trove-api-2016.1.0-20161230.el7.noarch
openstack-utils-2014.2-1.el7.noarch
openstack-nova-cells-2015.1.0-20170106.el7.noarch
openstack-nova-network-2015.1.0-20170106.el7.noarch
openstack-keystone-2015.1.0-20161230.el7.noarch
openstack-nova-console-2015.1.0-20170106.el7.noarch
openstack-neutron-ml2-2015.1.0-20161230.el7.noarch
openstack-trove-taskmanager-2016.1.0-20161230.el7.noarch
openstack-nova-common-2015.1.0-20170106.el7.noarch
openstack-nova-novncproxy-2015.1.0-20170106.el7.noarch
openstack-neutron-2015.1.0-20161230.el7.noarch
```

```bash
[root@RGCC01 ~]# rpm -qa |grep qemu
qemu-common-1.6.1-11.el7.centos.x86_64
qemu-kvm-1.6.1-11.el7.centos.x86_64
libvirt-daemon-driver-qemu-1.2.8-17.el7.centos.2.x86_64
qemu-img-1.6.1-11.el7.centos.x86_64
ipxe-roms-qemu-20130517-6.gitc4bce43.el7.noarch
```

```bash
[root@RGCC01 ~]# cat /etc/cinder/cinder.conf | grep zbs
enabled_backends=zbs
[zbs]
volume_driver=cinder.volume.drivers.zbs.ZBSDriver
zbs_chunk_ip=192.168.18.32
```

删除全部vm

cinder list | grep -o '[0-9a-fA-F]\{8\}-[0-9a-fA-F]\{4\}-[0-9a-fA-F]\{4\}-[0-9a-fA-F]\{4\}-[0-9a-fA-F]\{12\}' | xargs cinder delete

nova list | grep -o '[0-9a-fA-F]\{8\}-[0-9a-fA-F]\{4\}-[0-9a-fA-F]\{4\}-[0-9a-fA-F]\{4\}-[0-9a-fA-F]\{12\}' | xargs nova delete

### Taking the binaries as are

You can use this method to lift the actual executables from one system and tar them up for deployment on another system.

*machine A*

```
$ rpm -ql <packageNameHere> | xargs tar -zcvf /tmp/program.tgz

```

*machine B*

```
$ tar -zxvf /path/to/your/program.tgz
```

```jsx
for i in $(rpm -qa | grep qemu)
do
echo $i.tgz
rpm -ql $i | xargs tar -zcvf $i.tgz
done

for i in $(rpm -qa | grep libvirt)
do
echo $i.tgz
rpm -ql $i | xargs tar -zcvf $i.tgz
done

rpm -ql bluez-libs-4.101-13.el7.x86_64 | xargs tar -zcvf bluez-libs-4.101-13.el7.x86_64.tgz

i=brlapi-0.6.0-9.el7.x86_64; rpm -ql $i | xargs tar -zcvf $i.tgz

for i in $(ll /root/libvirt-* | awk '{print $9}')
do
echo $i
tar -zxvf $i
done

```

[How to build an RPM package from the installed files?](https://unix.stackexchange.com/questions/140778/how-to-build-an-rpm-package-from-the-installed-files)

[0010608: Could not access KVM kernel module: Permission denied - CentOS Bug Tracker](https://bugs.centos.org/view.php?id=10608)

selinux 0

[OpenStack虚拟机挂载数据卷过程分析](https://www.jingh.top/2017/09/08/OpenStack%E8%99%9A%E6%8B%9F%E6%9C%BA%E6%8C%82%E8%BD%BD%E6%95%B0%E6%8D%AE%E5%8D%B7%E8%BF%87%E7%A8%8B%E5%88%86%E6%9E%90/)

一共三处相关的东西：




前俩是一个问题



这个inject data 一般有三种， 通过nova配置项来配置开关

file

admin password 测试通过

ssh pub key 测试通过

rebuid and change admin password ok



```bash
wget http://192.168.17.20/repo/pub/smartxos/iso/4.0.14/SMTXOS-4.0.14-el7-2209281427-x86_64.iso
[root@ECM03 ~]# cat /etc/yum.repos.d/smartx.repo 
[rhel-local]
name=local
baseurl=file:///mnt
enabled=1
gpgcheck=0
                            x86_64/
[root@ECM03 ~]# mount ./SMTXOS-4.0.14-el7-2209281427-x86_64.iso /mnt
mount: /dev/loop0 is write-protected, mounting read-only
```

[自定义centos系统镜像](https://juejin.cn/post/6952418144510181389)

[搭建repo服务器](https://docs.openeuler.org/zh/docs/20.09/docs/Administration/%E6%90%AD%E5%BB%BArepo%E6%9C%8D%E5%8A%A1%E5%99%A8.html)

```bash
689  mount -t cgroup -o cpuset none /sys/fs/cgroup/cpuset
691  mount -t cgroup -o cpu none /sys/fs/cgroup/cpu
```

createrepo zbs_upgrade_test_iso/mnt/
mkisofs -r -o zbs_upgrade_test.iso ./zbs_upgrade_test_iso/mnt/
scp zbs_upgrade_test.iso [root@192.168.17.20](mailto:root@192.168.17.20):/data/distros/gouxi/zbs20xupgrade

nova boot --flavor m1.tiny --block-device id=c74d6d74-2c3b-4efb-9da2-f9b07806437f,source=image,dest=volume,size=1,bootindex=0

嵌套环境启动vm

nova boot --flavor fcbf7ec2-0949-4f1d-b915-8e31fa1a15e1 --block-device id=e0009911-f742-47b2-826b-9bacd0b4b1e6,source=image,dest=volume,size=1,bootindex=0 --nic net-id=021bfee1-71bc-4c50-ac50-ee02a755377b --availability-zone az01.cell01.virt test

北京 IDC	supermicro	功能测试	管理IP：192.168.18.31   IPMI 192.168.18.131
北京 IDC	supermicro	功能测试	管理IP：192.168.18.32  IPMI  192.168.18.132
北京 IDC	supermicro	功能测试	管理IP：192.168.18.33  IPMI  192.168.18.133
北京 IDC	supermicro	功能测试	管理IP：192.168.18.34  IPMI  192.168.18.134

管理：192.168.18.31
计算和存储：192.168.18.32-34
root/3QTOtCP7jkw+sx9Y

联通环境启动vm 

嵌套环境启动vm

nova boot --flavor ec6aeacb-7301-451a-8ed1-f047c701bea4 --block-device id=9be5735e-9dcd-4cdb-9f29-f3ad303c66c7,source=image,dest=volume,size=50,bootindex=0 --nic net-id=c2730660-f2eb-400a-895b-91d490dacf06 --availability-zone az01.cell01.LANZHOU test

nova boot --flavor  ec6aeacb-7301-451a-8ed1-f047c701bea4 --block-device id=173d022b-2845-4649-b924-3e688705ae06,source=image,dest=volume,size=20,bootindex=0 --nic net-id=c2730660-f2eb-400a-895b-91d490dacf06 --availability-zone az01.cell01.LANZHOU test

nova boot --flavor m1.tiny --block-device id=fca4749a-b96b-440c-b642-9c108f6933bd,source=image,dest=volume,size=1,bootindex=0 --nic net-id=80746f01-6ff5-40fb-ac9f-d2ce5fdc9c74 --availability-zone nova test

iptables -I INPUT -s XXX.XXX.XXX.XXX -j DROP

`python -m SimpleHTTPServer 80`

创建1000 volume 

```bash
for i in {1..1000}
do 
echo $(uuidgen)
sudo zbs-meta volume create default $(uuidgen) 1
done
```

# NVMF

```bash
[rocky]
name=el75-rocky
baseurl=https://mirrors.bfsu.edu.cn/centos-vault/7.5.1804/cloud/x86_64/openstack-rocky/
enabled=1

[virt]
name=kvm-common
baseurl=https://mirrors.bfsu.edu.cn/centos-vault/7.4.1708/virt/x86_64/kvm-common/
gpgcheck=0
enabled=1

[centos-7]
name=centos-7
baseurl=http://192.168.17.20/repo/pub/centos/7/os/x86_64/
gpgcheck=0
enabled=1

```

```bash
# 8.4
[openstack]
name=openstack
baseurl=https://mirrors.bfsu.edu.cn/centos-vault/8.4.2105/cloud/x86_64/openstack-rocky/
enabled=1

[virt]
name=kvm-common
baseurl=https://mirrors.bfsu.edu.cn/centos-vault/8.4.2105/virt/x86_64/kvm-common/
gpgcheck=0
enabled=1

```

```bash
systemctl stop NetworkManager.service
systemctl disable NetworkManager.service
systemctl stop firewalld
systemctl disable firewalld

# 关闭selinux 
setenforce 0 ; sed -i 's/=enforcing/=disabled/g' /etc/sysconfig/selinux

yum install -y qemu-kvm libvirt-client libvirt-daemon libvirt-daemon-driver-qemu python-setuptools
yum install -y libvirt-client libvirt-daemon libvirt-daemon-driver-qemu 
```

```bash
# zed
packstack --allinone --provision-image-url=http://192.168.25.178:8081/images/cirros-0.3.4-x86_64-disk.img --os-swift-install=n --os-ceilometer-install=n
```

- On RHEL 8:
    
    `$ sudo dnf install -y https://www.rdoproject.org/repos/rdo-release.el8.rpm
    $ sudo dnf update -y
    $ subscription-manager repos --enable codeready-builder-for-rhel-8-x86_64-rpms
    $ sudo dnf install -y openstack-packstack
    $ sudo packstack --allinone`
    

[NVMe over Fabric | SLES 15](https://documentation.suse.com/zh-cn/sles/15-GA/html/SLES-all/cha-nvmeof.html)

[How to get high performance for network connected solid-state drives with NVMe over fabrics - Superuser](https://superuser.openinfra.dev/articles/how-to-get-high-performance-for-network-connected-solid-state-drives-with-nmve-over-fabrics/)

[NVMeoF Multipathing — Cinder Specs 0.0.1.dev636 documentation](https://specs.openstack.org/openstack/cinder-specs/specs/yoga/nvme-multipath.html)

[https://docs.google.com/presentation/d/1lPU8mQ7jJmr9Tybu5gXkbE7NC1ppkMnoBS4cgSFhzWc/edit](https://docs.google.com/presentation/d/1lPU8mQ7jJmr9Tybu5gXkbE7NC1ppkMnoBS4cgSFhzWc/edit)

[OpenStack Docs: DevStack](https://docs.openstack.org/devstack/rocky/)

devstack 搭建：

```bash

# ubuntu 22.04

sudo useradd -s /bin/bash -d /opt/stack -m stack

sudo chmod +x /opt/stack

echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo -u stack -i

git clone https://opendev.org/openstack/devstack
cd devstack
cp samples/local.conf .
./stack.sh
```

[CentOS中使用Packstack(RDO)快速部署OpenStack单节点实验环境](https://www.koenli.com/5c034e4f.html#安装Packstack)

[透過 Kolla-Ansible 跟 Container 部署 OpenStack - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10271837)

[DevStack](https://newptone.gitbook.io/deployopenstackwithpuppet/deployment_tool/devstack)

[4.11 Creating a Cinder Volume Fails Due to Missing Default Volume Group](https://docs.oracle.com/cd/E64747_01/E68930/html/osrns-issues-21954155.html)

[Bug #1592356 “cinder-api exception due to default_volume_type no...” : Bugs : kolla](https://bugs.launchpad.net/kolla/+bug/1592356)

[openstack neutron 错误](https://youngwanjin.github.io/2020/08/10/openstack-neutron-error/#neutron-agent-list-没有-nova-上的-agent)

wget -O /etc/yum.repos.d/CentOS-Base.repo https://repo.huaweicloud.com/repository/conf/CentOS-8-reg.repo

[专家教你纯手工部署 OpenStack 最小系统-51CTO.COM](https://www.51cto.com/article/478713.html)

[OpenStack虚拟机挂载数据卷过程分析_成字第0431879的博客-CSDN博客](https://blog.csdn.net/weixin_41738417/article/details/93853967?ydreferer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8=)

[cinder创建卷源码流程分析](https://www.jianshu.com/p/9266d8a5ab16)

[http://192.168.17.20/GuestOS/cirros/cirros-0.5.2-x86_64-disk.img](http://192.168.17.20/GuestOS/cirros/cirros-0.5.2-x86_64-disk.img)


[Site Unreachable](https://www.reddit.com/r/openstack/comments/ywb68e/stop_using_packstack_or_devstack_installation_if/)

## Minimal deployment for Zed[¶](https://docs.openstack.org/install-guide/openstack-services.html#minimal-deployment-for-zed "Permalink to this heading")

At a minimum, you need to install the following services. Install the services in the order specified below:

- Identity service – [keystone installation for Zed](https://docs.openstack.org/keystone/zed/install/)
    
- Image service – [glance installation for Zed](https://docs.openstack.org/glance/zed/install/)
    
- Placement service – [placement installation for Zed](https://docs.openstack.org/placement/zed/install/)
    
- Compute service – [nova installation for Zed](https://docs.openstack.org/nova/zed/install/)
    
- Networking service – [neutron installation for Zed](https://docs.openstack.org/neutron/zed/install/)
    

We advise to also install the following components after you have installed the minimal deployment services:

- Dashboard – [horizon installation for Zed](https://docs.openstack.org/horizon/zed/install/)
    
- Block Storage service – [cinder installation for Zed](https://docs.openstack.org/cinder/zed/install/)

[Site Unreachable](https://linuxhint.com/install-configure-openstack-rocky-linux-9/)

[How To Install MariaDB on Rocky Linux 9 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-rocky-linux-9)
[How to Install RabbitMQ on Rocky Linux 9 - Distroid](https://distroid.net/how-to-install-rabbitmq-on-rocky-linux-9/)
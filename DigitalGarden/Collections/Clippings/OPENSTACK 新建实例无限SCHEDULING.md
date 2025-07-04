---
title: "[OPENSTACK] 新建实例无限SCHEDULING"
source: "https://www.cnblogs.com/xzlive/p/14942406.html"
author:
  - "[[MR__Wang]]"
published: 2021-06-27
created: 2025-07-03
description: "症状： 创建实例时一直处在build状态，无限scheduling，且无论多长时间Horizon都不显示任何错误 neutron-server.log无ERROR或WARNING 出现这种情况还有可能是其他原因导致的，请根据自己的情况和以下的日志来判断。 原因： nova-conductor无法连接"
tags:
  - "clippings"
---

### 症状：

- 创建实例时一直处在build状态，无限scheduling，且无论多长时间Horizon都不显示任何错误
- neutron-server.log无ERROR或WARNING

出现这种情况还有可能是其他原因导致的，请根据自己的情况和以下的日志来判断。

### 原因：

- nova-conductor无法连接到rabbitmq消息队列，导致其无法与其他组件联络，无法控制nova-compute部署实例。
- nova-conductor虽然进程显示配置文件为/etc/nova/nova.conf，但是实际上不从nova.conf中读取配置，nova-conductor直接从mysql数据库中读取自己的配置。

### 诊断过程：

一般来说我们遇到Horizon创建实例的时候，如果长时间卡在Scheduling或者Spawning，最后会扔一个错误，在我接触过的案例中，十有八九都是neutron-server的锅，然而这次不是。

- 按照我以往的经验去寻找neutron-server.log中的错误讯息，结果一无所获，只找到了以下这一条消息：

`2018-01-23 16:16:20.657 18341 WARNING keystonemiddleware.auth_token [-] A valid token was submitted as a service token, but it was not a valid service token. This is incorrect but backwards compatible behaviour. This will be removed in future releases.`

但这并不是问题的原因，这条警告讯息是OpenStack的一个bug，可自行Google，在devstack中已没有这条警告讯息。

此时已经可以排除neutron-server的问题了，没有更多的警告消息，且当前警告消息无用

- 接下来能做的就是寻找其他组件的日志，来寻找问题
- 实际上我在这里犯了个错误
- 因为是远程调试，延迟比较大，所以我就直接tail了nova各个组件的日志，又因为我更改systemctl restart nova-\*.service，所以没有看到任何日志。
- nova-conductor这个组件又比较有意思，他启动的时候不会验证当前配置，他启动的时候只会打印出以下日志，所以我忽略了

`# ...`  
`2018-01-23 06:40:16.981 8090 INFO nova.service [-] Starting conductor node (version 16.0.3)`  
`2018-01-23 06:40:16.991 8086 INFO nova.service [-] Starting conductor node (version 16.0.3)`  
`2018-01-23 06:40:16.995 8091 INFO nova.service [-] Starting conductor node (version 16.0.3)`  
`2018-01-23 06:40:16.996 8092 INFO nova.service [-] Starting conductor node (version 16.0.3)`  
`# End`

后来一个一个log文件cat，的确是看到这个了，去检查了一遍nova.conf和neutron.conf，发现rabbitmq凭据配置无误，就跳过了

最后重新启动一个实例，重新tail nova\* 才发现了nova-conductor又报这个错误，这就很有意思了，错误如下：

`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server [req-eb9ca3b8-58d8-4103-b8b0-d5b9a3d706b1 244f2e998d2e424d8da726ccec82588b 74ff9f41f424407d99df3e6607de120f - default default] Exception during message handling: AccessRefused: (0, 0): (403) ACCESS_REFUSED - Login was refused using authentication mechanism AMQPLAIN. For details see the broker logfile.`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server Traceback (most recent call last):`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/rpc/server.py", line 160, in _process_incoming`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server res = self.dispatcher.dispatch(message)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/rpc/dispatcher.py", line 213, in dispatch`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server return self._do_dispatch(endpoint, method, ctxt, args)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/rpc/dispatcher.py", line 183, in _do_dispatch`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server result = func(ctxt, **new_args)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/nova/conductor/manager.py", line 1172, in schedule_and_build_instances`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server limits=host['limits'])`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/nova/compute/rpcapi.py", line 1124, in build_and_run_instance`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server limits=limits)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/rpc/client.py", line 147, in cast`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server self.transport._send(self.target, msg_ctxt, msg, retry=self.retry)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/transport.py", line 123, in _send`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server timeout=timeout, retry=retry)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/amqpdriver.py", line 578, in send`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server retry=retry)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/amqpdriver.py", line 539, in _send`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server with self._get_connection(rpc_common.PURPOSE_SEND) as conn:`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/amqpdriver.py", line 494, in _get_connection`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server purpose=purpose)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/common.py", line 399, in __init__`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server self.connection = connection_pool.get()`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/pool.py", line 107, in get`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server return self.create()`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/pool.py", line 144, in create`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server return self.connection_cls(self.conf, self.url, purpose)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/impl_rabbit.py", line 614, in __init__`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server self.ensure_connection()`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/impl_rabbit.py", line 713, in ensure_connection`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server self.ensure(method=self.connection.connect)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/oslo_messaging/_drivers/impl_rabbit.py", line 807, in ensure`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server ret, channel = autoretry_method()`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/kombu/connection.py", line 494, in _ensured`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server return fun(*args, **kwargs)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/kombu/connection.py", line 569, in __call__`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server self.revive(self.connection.default_channel)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/kombu/connection.py", line 819, in default_channel`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server self.connection`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/kombu/connection.py", line 802, in connection`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server self._connection = self._establish_connection()`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/kombu/connection.py", line 757, in _establish_connection`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server conn = self.transport.establish_connection()`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/kombu/transport/pyamqp.py", line 130, in establish_connection`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server conn.connect()`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/amqp/connection.py", line 300, in connect`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server self.drain_events(timeout=self.connect_timeout)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/amqp/connection.py", line 464, in drain_events`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server return self.blocking_read(timeout)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/amqp/connection.py", line 469, in blocking_read`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server return self.on_inbound_frame(frame)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/amqp/method_framing.py", line 68, in on_frame`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server callback(channel, method_sig, buf, None)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/amqp/connection.py", line 473, in on_inbound_method`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server method_sig, payload, content,`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/amqp/abstract_channel.py", line 142, in dispatch_method`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server listener(*args)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server File "/usr/lib/python2.7/dist-packages/amqp/connection.py", line 595, in _on_close`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server (class_id, method_id), ConnectionError)`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server AccessRefused: (0, 0): (403) ACCESS_REFUSED - Login was refused using authentication mechanism AMQPLAIN. For details see the broker logfile.`  
`2018-01-23 06:26:18.952 7478 ERROR oslo_messaging.rpc.server`

nova-conductor在连接到RabbitMQ消息队列的时候，被拒绝访问了

`AccessRefused: (0, 0): (403) ACCESS_REFUSED - Login was refused using authentication mechanism AMQPLAIN. For details see the broker logfile.`

根据这个提示我们来找rabbitmq的日志，rabbit@controller.log

发现其中有如下错误信息

`=ERROR REPORT==== 23-Jan-2018::04:11:23 ===`  
`closing AMQP connection <0.6917.0> (10.0.0.30:59652 -> 10.0.0.30:5672):`  
`{handshake_error,starting,0,`  
`{amqp_error,access_refused,`  
`"AMQPLAIN login refused: user 'openstack' - invalid credentials",`  
`'connection.start_ok'}}`

`=INFO REPORT==== 23-Jan-2018::04:29:59 ===`  
`accepting AMQP connection <0.7038.0> (10.0.0.30:34068 -> 10.0.0.30:5672)`

`=INFO REPORT==== 23-Jan-2018::04:30:00 ===`  
`accepting AMQP connection <0.7055.0> (10.0.0.30:34096 -> 10.0.0.30:5672)`

`=ERROR REPORT==== 23-Jan-2018::04:30:03 ===`  
`closing AMQP connection <0.7055.0> (10.0.0.30:34096 -> 10.0.0.30:5672):`  
`{handshake_error,starting,0,`  
`{amqp_error,access_refused,`  
`"AMQPLAIN login refused: user 'openstack' - invalid credentials",`  
`'connection.start_ok'}}`

到这里问题就很明了了，但是我们仍然不知道为什么会拒绝连接，上面说到过的，检查nova.conf和neutron.conf没有任何问题，rabbitmq连接测试正常

已经没有招了。

### 问题根源/解决问题：

如果你 `systemctl status nova-conductor.service` ， 你会发现他有指向配置文件 ` --config-file=/etc/nova/nova.conf`

`● nova-conductor.service - OpenStack Compute Conductor`  
`Loaded: loaded (/lib/systemd/system/nova-conductor.service; enabled; vendor preset: enabled)`  
`Active: active (running) since Tue 2018-01-23 06:40:14 PST; 1 day 13h ago`  
`Main PID: 8036 (nova-conductor)`  
`Tasks: 25`  
`Memory: 1.8G`  
`CPU: 1h 32min 57.730s`  
`CGroup: /system.slice/nova-conductor.service`  
`├─8036 /usr/bin/python /usr/bin/nova-conductor --config-file=/etc/nova/nova.conf --log-file=/var/log/nova/nova-conductor.log`  
`├─......`  
`└─8098 /usr/bin/python /usr/bin/nova-conductor --config-file=/etc/nova/nova.conf --log-file=/var/log/nova/nova-conductor.log`

`Jan 23 06:40:14 controller systemd[1]: Starting OpenStack Compute Conductor...`  
`Jan 23 06:40:14 controller systemd[1]: Started OpenStack Compute Conductor.`

但是他并不从nova.conf中读取 `transport_url` ！！！！

他只从nova.conf中读取 `connection = mysql+pymysql:` 字段，获取到连接数据库的凭据，然后从数据库读取配置！！

WTF？？？（我相信大部分人和我的反应是一样的）

实际上如果小心翼翼的按照教程一步一步来或许就不会出现这样的问题，在配置nova的时候，会有一段往mysql数据库中写入数据，然后sync\_db的配置步骤

这一步中，有一句

`su -s /bin/bash nova -c "nova-manage cell_v2 create_cell --name cell1 \`  
`--database_connection mysql+pymysql://nova:password@10.0.0.30/nova \`  
`--transport-url rabbit://openstack:password@10.0.0.30:5672"`

如果这里写错了transport-url，那就完蛋了，因为他直接把这个写到数据库里了

但是幸运的是，他明文存储的密码和用户名，那就让我们来跑一下联合查询，先以root身份登陆mysql server，然后执行以下语句

此语句是从数据库中查找’openstack’字段，当然这已经不算是个联合查询了，因为我已经知道了nova\_api.cell\_mappings了，直接从这里面找即可

``SELECT * FROM `nova_api`.`cell_mappings` WHERE `created_at` LIKE BINARY '%openstack%' OR `updated_at` LIKE BINARY '%openstack%' OR `id` LIKE BINARY '%openstack%' OR `uuid` LIKE BINARY '%openstack%' OR `name` LIKE BINARY '%openstack%' OR `transport_url` LIKE BINARY '%openstack%' OR `database_connection` LIKE BINARY '%openstack%' ;``

永远不要尝试在大规模生产环境中执行联合查询！！

结果如下

`+---------------------+------------+----+--------------------------------------+-------+--------------------------------------------------------+----------------------------------------------------------+`  
`| created_at | updated_at | id | uuid | name | transport_url | database_connection |`  
`+---------------------+------------+----+--------------------------------------+-------+--------------------------------------------------------+----------------------------------------------------------+`  
`| 2018-01-23 08:37:50 | NULL | 2 | 4952f0f2-***** | cell1 | rabbit://openstack:******@10.0.0.30:5672 | mysql+pymysql://nova:********@10.0.0.30/nova |`  
`+---------------------+------------+----+--------------------------------------+-------+--------------------------------------------------------+----------------------------------------------------------+`  
`1 row in set (0.00 sec)`

发现 `rabbit://openstack:******@10.0.0.30:5672` 的密码是错误的，修改即可

来源：https://blog.silversky.moe/works/openstack-lanuch-instance-infinite-scheduling

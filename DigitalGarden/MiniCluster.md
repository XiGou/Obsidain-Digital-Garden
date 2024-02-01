chunk_options.instance_id = id_;

chunk_options.rpc_addr = "127.0.0.1";

chunk_options.rpc_port = GetPort();

chunk_options.grpc_port = GetPort() + 6;

chunk_options.data_channel_addr = "127.0.0.1";

chunk_options.data_channel_port = GetPort() + 1;

chunk_options.http_port = GetPort() + 2;

chunk_options.secondary_data_ip = "127.0.0.1";

chunk_options.scvm_mode_host_data_ip = "";

chunk_options.base_dir = base_dir_;

chunk_options.machine_uuid = "test_machine_" + ToString(id_);

chunk_options.thctx = chunk_thctx;

chunk_options.perf_rpc_port = GetPort() + 7;

chunk_options.lsm = lsm_;

chunk_options.meta = meta_;

chunk_options.follower = mock_follower_holder_.get();

chunk_options.use_rdma = use_rdma_;

chunk_options.nvmf_rdma_port = GetPort() + 3;

chunk_options.nvmf_tcp_port = GetPort() + 4;

int GetPort(int idx) { return 10300 + idx * 50; }
meta_proxy_ = new libmeta::MetaProxy("127.0.0.1", GetPort() + 8,

FLAGS_zk_hosts, loop_, options);


```bash
[root@25adf4729b52 examples]# netstat -anop | grep LISTEN | grep 10301
tcp        0      0 0.0.0.0:10301           0.0.0.0:*               LISTEN      41587/src/zbs_clust  off (0.00/0/0)
[root@25adf4729b52 examples]# netstat -anop | grep LISTEN | grep 10308
tcp        0      0 127.0.0.1:10308         0.0.0.0:*               LISTEN      41587/src/zbs_clust  off (0.00/0/0)
```

```Dockerfile
FROM registry.smtx.io/zbs/zbs-buildtime:el7-x86_64

  
  

ADD ./el7/zbs_cluster /bin/zbs_cluster

ADD ./el7/init_test_env.sh /bin/init_test_env.sh

WORKDIR /zbs

EXPOSE 10301 10308

CMD [ "/bin/init_test_env.sh"]
```


```bash
#!/usr/bin/bash

#

# Copyright (C) 2013-2023 SmartX

# All rights reserved.

#

  

# preparation

mkdir -p /etc/zbs

mkdir -p /var/log/zbs

mkdir -p /var/lib/zbs/chunkd

mkdir -p /var/lib/zbs/iscsid

mkdir -p /var/lib/zbs/metad

mkdir -p /var/lib/zbs/registry

mkdir -p /var/lib/zbs/zbs-nfsd

chmod -R 1777 /var/log/zbs /var/lib/zbs

  

# create config file

cat > /etc/zbs/zbs.conf <<  EOF

[network]

data_ip=127.0.0.1

heartbeat_ip=127.0.0.1

vm_ip=127.0.0.1

web_ip=127.0.0.1

  

[cluster]

role=standalone

members=127.0.0.1

zookeeper=127.0.0.1:2181

mongo=127.0.0.1:27017

EOF

cp -v /etc/zookeeper/zoo_zbs.cfg /etc/zookeeper/zoo.cfg

sed -i 's/cnxTimeout=2/cnxTimeout=10/g' /etc/zookeeper/zoo.cfg

  

# start rpcbind

/sbin/rpcbind -w

  

# start zookeeper

zkServer.sh stop

rm -rf /var/log/zookeeper

mkdir -p /var/log/zookeeper

chown -R zookeeper:zookeeper /var/log/zookeeper

zkServer.sh start || die "Failed to start zookeeper"

sleep 3

/bin/zbs_cluster
```
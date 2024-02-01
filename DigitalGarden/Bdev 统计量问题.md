```python
  

def get_speed(self, v1, v2, field, interval):

	delta = v2[field] - v1[field]
	if delta < 0:
	# overflow, the data source is uint64_t.
	delta = delta % (1 << 64)
	return delta / interval
```

```
#!/bin/bash

# 设置DPDK服务器的主机名或IP地址和端口号
server="your-dpdk-server"
port="1234"

# 设置JSON-RPC请求的参数
bdev_name="your-bdev-name"

# 构建JSON-RPC请求体
request='{
  "jsonrpc": "2.0",
  "method": "bdev_get_iostat",
  "params": {
    "bdev_name": "'"${bdev_name}"'"
  },
  "id": 1
}'

# 发送HTTP POST请求
curl -X POST -H "Content-Type: application/json" -d "$request" "http://$server:$port/RPC2"
```

python -c "from zbs.perf.client import get_local_volumes_perf;print(get_local_volumes_perf());" 

python -c "from zbs.perf.client import get_local_volumes_perf;print(get_local_volumes_perf());" 

 yum install http://192.168.17.20/repo/pub/epel/7/x86_64/j/jq-1.5-1.el7.x86_64.rpm http://192.168.17.20/repo/pub/epel/7/x86_64/o/oniguruma-5.9.5-3.el7.x86_64.rpm 安装jq

```bash
#!/bin/bash

while true; do
    output=$( python -c "from zbs.perf.client import SPDKBdevPerf;print(SPDKBdevPerf().get_bdev_perf(vm_uuid='dd3f885a-66de-4fe1-a820-45d44dc4e0e4'));" |awk -F', ' -v RS=',' '/num_write_ops/ {print $1}')
    echo "$output"
    sleep 1
done
```

```bash
[root@node131-232 15:50:32 ~]$ ./gouxi_bdev.sh 
{u'bdevs': [{u'num_write_ops': 0
 {u'num_write_ops': 1152868
 {u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 2353
{u'bdevs': [{u'num_write_ops': 0
 {u'num_write_ops': 1158075
 {u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 2357
{u'bdevs': [{u'num_write_ops': 0
 {u'num_write_ops': 1165470
 {u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 2357
{u'bdevs': [{u'num_write_ops': 0
 {u'num_write_ops': 1171273
 {u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 2359
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/usr/lib/python2.7/site-packages/zbs/perf/client.py", line 130, in __init__
    self.client = JSONRPCClient(rpc_domain_path, 4420, 60)
  File "/usr/lib/python2.7/site-packages/zbs/perf/jsonrpc.py", line 46, in __init__
    self._connect(addr, port)
  File "/usr/lib/python2.7/site-packages/zbs/perf/jsonrpc.py", line 74, in _connect
    raise JSONRPCException("Error while connecting to %s\n" "Error details: %s" % (addr, ex))
zbs.perf.jsonrpc.JSONRPCException


{u'bdevs': [{u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 0
{u'bdevs': [{u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 0
 {u'num_write_ops': 0
```
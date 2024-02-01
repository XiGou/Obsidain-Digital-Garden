```python
    packet = self.recv(size - cur_len)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/gevent/_socketcommon.py", line 663, in recv
    self._wait(self._read_event)
  File "src/gevent/_hub_primitives.py", line 317, in gevent._gevent_c_hub_primitives.wait_on_socket
  File "src/gevent/_hub_primitives.py", line 322, in gevent._gevent_c_hub_primitives.wait_on_socket
  File "src/gevent/_hub_primitives.py", line 297, in gevent._gevent_c_hub_primitives._primitive_wait
gevent.exceptions.ConcurrentObjectUseError: This socket is already used by another greenlet: <bound method Waiter.switch of <gevent._gevent_c_waiter.Waiter object at 0x7f6890e36cf0>>
2023-09-14 10:22:53 [1113989] [ERROR] Exception occurs
Traceback (most recent call last):
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs/lib/rpc/channel.py", line 160, in parseResponse
    response.ParseFromString(byte_stream)
google.protobuf.message.DecodeError: Error parsing message with type 'zbs.meta.SnapshotsResponse'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/flask/app.py", line 1820, in full_dispatch_request
    rv = self.dispatch_request()
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/flask/app.py", line 1796, in dispatch_request
    return self.ensure_sync(self.view_functions[rule.endpoint])(**view_args)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/schema_sugar/contrib/flask_sugar.py", line 41, in resource
    return api_function(request.method, data, web_request=request, **kwargs)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/schema_sugar/__init__.py", line 650, in resources_api
    return self._api_run(operation, data, web_request, **kwargs)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/schema_sugar/__init__.py", line 666, in _api_run
    result = self.process(operation, data, web_request, **kwargs)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/schema_sugar/__init__.py", line 642, in process
    return getattr(self, operation)(processed_data, web_request, **kwargs)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs_rest/api/common/utils.py", line 99, in decorated_view
    return func(*args, user=user, **kwargs)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs_rest/api/v2/nfs/view.py", line 340, in index
    snapshots_counts = self.nfs_client.get_file_snapshots_count(export_name)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs/nfs/client.py", line 845, in get_file_snapshots_count
    snapshots = self.snapshot_list(export_name=export_name, inode_id=file_inode_id).snapshots
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs/lib/utils/__init__.py", line 187, in _func
    return func(self, *args, **kwargs)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs/lib/utils/__init__.py", line 365, in __encode
    return func(*new_args, **kwargs)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs/nfs/client.py", line 622, in snapshot_list
    return ProtoStrWrap(self.nfs_service.ListSnapshot(None, request, None))
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/google/protobuf/service_reflection.py", line 278, in <lambda>
    self._StubMethod(inst, method, rpc_controller, request, callback))
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/google/protobuf/service_reflection.py", line 293, in _StubMethod
    return stub.rpc_channel.CallMethod(
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs/lib/rpc/channel.py", line 216, in CallMethod
    response = self.parseResponse(byte_stream, resp_class)
  File "/usr/local/venv/pyzbs/lib/python3.10/site-packages/zbs/lib/rpc/channel.py", line 162, in parseResponse
    raise error.BadResponseProtoError(
zbs.lib.rpc.error.BadResponseProtoError: Invalid response                                               (decodeError): Error parsing message with type 'zbs.meta.SnapshotsResponse'
```

复现命令：
```bash
[smartx@gouxi-505-el7-x86-64X20230824172350X2 13:01:40 ~]$ python3 rest_test.py  127.0.0.1 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 20 &&  grep UNK zbs_rest_test.log 
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
[smartx@gouxi-505-el7-x86-64X20230824172350X2 13:01:42 ~]$ python3 rest_test.py  127.0.0.1 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652 --concurrency 20 &&  grep UNK zbs_rest_test.log 
[smartx@gouxi-505-el7-x86-64X20230824172350X2 13:02:02 ~]$ 

```

为什么只有 nfs 出错？
```bash
[smartx@gouxi-505-el7-x86-64X20230824172350X2 13:44:49 ~]$ python3 rest_test.py  127.0.0.1 /api/v2/iscsi/targets/d7b331f5-c21c-441e-99f3-957df95437ce --concurrency 50 &&  grep UNK zbs_rest_test.log 
[smartx@gouxi-505-el7-x86-64X20230824172350X2 13:44:58 ~]$ python3 rest_test.py  127.0.0.1 /api/v2/iscsi/targets/d7b331f5-c21c-441e-99f3-957df95437ce/luns --concurrency 50 &&  grep UNK zbs_rest_test.log 
[smartx@gouxi-505-el7-x86-64X20230824172350X2 13:45:09 ~]$ python3 rest_test.py  127.0.0.1 /api/v2/iscsi/targets/d7b331f5-c21c-441e-99f3-957df95437ce/luns --concurrency 50 &&  grep UNK zbs_rest_test.log 
[smartx@gouxi-505-el7-x86-64X20230824172350X2 13:45:12 ~]$ 

```



修复之前

 sudo  python3 rest_test.py 127.0.0.1 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 10 && grep "UNK" zbs_rest_test.log 
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  
```bash
  {
  "data": {},
  "ec": "REST_UNKNOWN_ERROR",
  "error": {
    "detail": "",
    "msg": "This socket is already used by another greenlet: <bound method Waiter.switch of <gevent._gevent_c_waiter.Waiter object at 0x7fc3b3275f80>>"
  }
}

{
  "data": {
    "inodes": [],
    "parent_hierarchy_inodes": [
      {
        "id": "1020f3c4-9469-4e86",
        "name": "1020f3c4-9469-4e86",
        "parent_id": "zbs"
      }
    ]
  },
  "ec": "EOK",
  "error": {}
}
```

2023-09-19 14:58:07 [154562] [WARNING] nfs 1 1695106687.2416627
2023-09-19 14:58:07 [154561] [WARNING] nfs 1 1695106687.2477386
2023-09-19 14:58:07 [154561] [WARNING] nfs 2 1695106687.2616377
2023-09-19 14:58:07 [154562] [WARNING] nfs 1 1695106687.2657464
2023-09-19 14:58:07 [154561] [WARNING] nfs 3 1695106687.2748652
2023-09-19 14:58:07 [154562] [WARNING] nfs 2 1695106687.28738
2023-09-19 14:58:07 [154562] [WARNING] nfs 2 1695106687.3008642
2023-09-19 14:58:07 [154562] [WARNING] nfs 3 1695106687.323766

```bash
[smartx@gouxi-505-el7-x86-64X20230824172350X2 15:41:45 ~]$ time sudo  python3 rest_test.py 127.0.0.1 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 100 && grep "UNK" zbs_rest_test.log 

real    0m7.648s
user    0m0.569s
sys     0m0.080s

[smartx@gouxi-505-el7-x86-64X20230824172350X2 15:42:59 ~]$ time sudo  python3 rest_test.py 10.233.156.236 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 100 

real    0m3.054s
user    0m0.659s
sys     0m0.094s
```



```bash
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4c10 object, fd=26, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4c10 object, fd=26, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c42e0 object, fd=22, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c42e0 object, fd=22, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61403b50 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61403b50 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402a40 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402a40 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61401000 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61401000 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61401000 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61401000 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c42e0 object, fd=22, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c42e0 object, fd=22, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [DEBUG] Closing connection. 
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61401000 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61401000 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402ff0 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402ff0 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402ff0 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402ff0 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61401000 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61401000 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402a40 object, fd=21, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402a40 object, fd=21, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c49e0 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c49e0 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402a40 object, fd=21, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf61402a40 object, fd=21, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c42e0 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c42e0 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4c10 object, fd=21, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4c10 object, fd=21, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c49e0 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c49e0 object, fd=20, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c42e0 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c42e0 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4c10 object, fd=21, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4c10 object, fd=21, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4970 object, fd=22, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4970 object, fd=22, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [DEBUG] Closing connection. 
2023-09-19 18:54:45 [159479] [DEBUG] Closing connection. 
2023-09-19 18:54:45 [159479] [WARNING] gouxi locked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4c10 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [WARNING] gouxi unlocked<zbs.lib.zbs_socket.TcpSocket at 0x7fcf612c4c10 object, fd=15, family=2, type=1, proto=0>
2023-09-19 18:54:45 [159479] [DEBUG] Closing connection. 
```


 不使用单例
```bash
[smartx@gouxi-505-el7-x86-64X20230824172350X2 09:49:54 ~]$ time sudo  python3 rest_test.py  127.0.0.1 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 50 && grep "UNK" zbs_rest_test.log 

real    0m2.390s
user    0m0.483s
sys     0m0.064s
[smartx@gouxi-505-el7-x86-64X20230824172350X2 09:50:04 ~]$ time sudo  python3 rest_test.py  10.233.156.238 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 50 && grep "UNK" zbs_rest_test.log 

real    0m1.887s
user    0m0.513s
sys     0m0.067s
```


修复前
```bash
[smartx@gouxi-505-el7-x86-64X20230824172350X2 15:21:01 ~]$ time sudo  python3 rest_test.py  192.168.24.141 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 100 && grep "UNK" zbs_rest_test.log 

real    0m2.674s
user    0m0.685s
sys     0m0.104s
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
[smartx@gouxi-505-el7-x86-64X20230824172350X2 15:34:38 ~]$ time sudo  python3 rest_test.py  192.168.24.141 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 50 && grep "UNK" zbs_rest_test.log 

real    0m1.458s
user    0m0.458s
sys     0m0.058s
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
  "ec": "REST_UNKNOWN_ERROR",
```
修复后：
```bash
[smartx@gouxi-505-el7-x86-64X20230824172350X2 15:35:37 ~]$ time sudo  python3 rest_test.py  192.168.24.141 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 50 && grep "UNK" zbs_rest_test.log 

real    0m1.761s
user    0m0.479s
sys     0m0.068s
[smartx@gouxi-505-el7-x86-64X20230824172350X2 16:55:21 ~]$ time sudo  python3 rest_test.py  192.168.24.141 /api/v2/nfs/exports/1020f3c4-9469-4e86-8d50-c8486b215652/inodes --concurrency 100 && grep "UNK" zbs_rest_test.log 

real    0m2.760s
user    0m0.657s
sys     0m0.079s
```

修复后的DEBUG 日志
```bash
2023-09-21 15:40:19 [105043] [DEBUG] patched SocketRpcChannel init: <zbs_rest.api.common.zbs_monkey_patch.PatchedSocketRpcChannel object at 0x7fc13b0c1e70> 
2023-09-21 15:40:19 [105043] [DEBUG] channel <zbs_rest.api.common.zbs_monkey_patch.PatchedSocketRpcChannel object at 0x7fc13b0c1e70> locked
2023-09-21 15:40:19 [105043] [DEBUG] GET /api/v2/storage/chunk
2023-09-21 15:40:19 [105043] [DEBUG] Create channel to 127.0.0.1:9999
2023-09-21 15:40:19 [105043] [DEBUG] patched SocketRpcChannel init: <zbs_rest.api.common.zbs_monkey_patch.PatchedSocketRpcChannel object at 0x7fc13b232470> 
2023-09-21 15:40:19 [105043] [DEBUG] channel <zbs_rest.api.common.zbs_monkey_patch.PatchedSocketRpcChannel object at 0x7fc13b232470> locked
2023-09-21 15:40:19 [105043] [DEBUG] channel <zbs_rest.api.common.zbs_monkey_patch.PatchedSocketRpcChannel object at 0x7fc13b0c1e70> unlocked
2023-09-21 15:40:19 [105043] [DEBUG] patched SocketRpcChannel init: <zbs_rest.api.common.zbs_monkey_patch.PatchedSocketRpcChannel object at 0x7fc13b0c1d20> 
2023-09-21 15:40:19 [105043] [DEBUG] patched SocketRpcChannel init: <zbs_rest.api.common.zbs_monkey_patch.PatchedSocketRpcChannel object at 0x7fc13b0c1e70> 
2023-09-21 15:40:19 [105043] [DEBUG] channel <zbs_rest.api.common.zbs_monkey_patch.PatchedSocketRpcChannel object at 0x7fc13b0c1e70> locked
```
https://github.com/gevent/gevent/issues/1009#issuecomment-319711592
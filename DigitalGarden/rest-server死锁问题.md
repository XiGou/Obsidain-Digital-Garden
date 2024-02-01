复现：
日志：
故障现场：
```
[smartx@SMTXOSSmokeAPIX20230805001647X3 14:22:26 ~]$ sudo /usr/local/venv/tuna/bin/pystack --include-greenlet 16012 | grep -E "logging\."
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.error("code={}, error_message={}".format(e.code(), e.details()))
    logging.exception("ZException occurs")
[smartx@SMTXOSSmokeAPIX20230805001647X3 14:22:34 ~]$ sudo /usr/local/venv/tuna/bin/pystack --include-greenlet 16012 | grep -E "logging\." | wc -l
27
```

然后随意发请求，因为有两个worker，一旦发生卡顿，可以观察到卡在logging的协程将会增加。
 ```
curl -X POST -H "Content-Type: application/json" -d '{"username": "root", "password": "111111"}' 127.0.0.1/api/v3/sessions
```
 
 
```
[smartx@SMTXOSSmokeAPIX20230805001647X3 14:28:11 ~]$ curl -X POST -H "Content-Type: application/json" -d '{"name": "string","replica_num": 0,"thin_provision": true,"description": "string","storage_pool_id": "string","whitelist": "string"}' http://127.0.0.1:10402/api/v2/nfs/exports
^C
[smartx@SMTXOSSmokeAPIX20230805001647X3 14:28:27 ~]$ sudo /usr/local/venv/tuna/bin/pystack --include-greenlet 16012 | grep -E "logging\." | wc -l
29
[smartx@SMTXOSSmokeAPIX20230805001647X3 14:28:35 ~]$ curl -X POST -H "Content-Type: application/json" -d '{"name": "string","replica_num": 0,"thin_provision": true,"description": "string","storage_pool_id": "string","whitelist": "string"}' http://127.0.0.1:10402/api/v2/nfs/exports
{
  "data": {},
  "ec": "REST_FORM_ERROR",
  "error": {
    "detail": {
      "replica_num": [
        "0 is not one of [1, 2, 3]"
      ]
    },
    "msg": "0 is not one of [1, 2, 3]\n\nFailed validating 'enum' in schema['properties']['replica_num']:\n    {'enum': [1, 2, 3], 'type': 'number'}\n\nOn instance['replica_num']:\n    0"
  }
}
[smartx@SMTXOSSmokeAPIX20230805001647X3 14:28:37 ~]$ curl -X POST -H "Content-Type: application/json" -d '{"name": "string","replica_num": 0,"thin_provision": true,"description": "string","storage_pool_id": "string","whitelist": "string"}' http://127.0.0.1:10402/api/v2/nfs/exports
^C
[smartx@SMTXOSSmokeAPIX20230805001647X3 14:28:42 ~]$ sudo /usr/local/venv/tuna/bin/pystack --include-greenlet 16012 | grep -E "logging\." | wc -l
30
```

curl -X POST -H "Content-Type: application/json" -H "X-SmartX-Token:f927b81b40b2406690e3e223811922b4" -d '{"name": "string","replica_num": 2,"thin_provision": true,"description": "string","storage_pool_id": "system","whitelist": ""}' http://127.0.0.1:10402/api/v2/nfs/exports


复现：
```
from gevent import monkey; monkey.patch_all()
import logging
from time import sleep
from zbs.lib.zk import Zookeeper

import logging
zk = Zookeeper()

class LogLockHandler(logging.StreamHandler):
    def __init__(self):
        self.zk = Zookeeper()
        logging.StreamHandler.__init__(self)
        
    def acquire(self):
        self.lock.acquire()
        print('Acquired logging lock')
        self.zk.__del__()
    def release(self):
        print('Released logging lock')
        self.lock.release()

handler = LogLockHandler()  
handler.setLevel(logging.INFO)

logger = logging.getLogger()
logger.addHandler(handler)
logger.setLevel(logging.INFO)

logger.info('Log message')
```


```
(zbs-client-py) [root@b54dedcf3ebe zbs-client-py]# ps -ef  |grep demo
root      38022   2552  0 09:06 pts/5    00:00:00 python deadlock/gevent_demo.py
root      38089  38024  0 09:07 pts/11   00:00:00 grep --color=auto demo
(zbs-client-py) [root@b54dedcf3ebe zbs-client-py]# pystack --include-greenlet 38022
Dumping Threads....


  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/gevent/hub.py", line 647, in run
    loop.run()
  File "<string>", line 1, in <module>
  File "<string>", line 1, in <module>

Dumping Greenlets....


  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/protocol/connection.py", line 512, in zk_loop
    if retry(self._connect_loop, retry) is STOP_CONNECTING:
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/retry.py", line 126, in __call__
    return func(*args, **kwargs)
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/protocol/connection.py", line 552, in _connect_loop
    status = self._connect_attempt(host, hostip, port, retry)
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/protocol/connection.py", line 617, in _connect_attempt
    self.logger.info('Closing connection to %s:%s', host, port)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 1477, in info
    self._log(INFO, msg, args, **kwargs)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 1624, in _log
    self.handle(record)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 1634, in handle
    self.callHandlers(record)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 1696, in callHandlers
    hdlr.handle(record)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 966, in handle
    self.acquire()
  File "/root/zbs-client-py/deadlock/gevent_demo.py", line 15, in acquire
    self.lock.acquire()
  File "/opt/smtx/python310/lib/python3.10/threading.py", line 168, in acquire
    rc = self._block.acquire(blocking, timeout)
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/gevent/thread.py", line 112, in acquire
    acquired = BoundedSemaphore.acquire(self, blocking, timeout)

---------------

  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/gevent/hub.py", line 647, in run
    loop.run()
  File "<string>", line 1, in <module>
  File "<string>", line 1, in <module>
  File "<string>", line 1, in <genexpr>

---------------

  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/handlers/gevent.py", line 78, in greenlet_worker
    func = queue.get()

---------------

  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/protocol/connection.py", line 512, in zk_loop
    if retry(self._connect_loop, retry) is STOP_CONNECTING:
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/retry.py", line 126, in __call__
    return func(*args, **kwargs)
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/protocol/connection.py", line 552, in _connect_loop
    status = self._connect_attempt(host, hostip, port, retry)
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/protocol/connection.py", line 592, in _connect_attempt
    s = self.handler.select([self._socket, self._read_sock],
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/handlers/gevent.py", line 129, in select
    return selector_select(*args, selectors_module=gevent.selectors,
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/handlers/utils.py", line 380, in selector_select
    ready = selector.select(timeout)
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/gevent/selectors.py", line 201, in select
    self._ready.wait(timeout)

---------------

  File "/root/zbs-client-py/deadlock/gevent_demo.py", line 29, in <module>
    logger.info('Log message')
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 1477, in info
    self._log(INFO, msg, args, **kwargs)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 1624, in _log
    self.handle(record)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 1634, in handle
    self.callHandlers(record)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 1696, in callHandlers
    hdlr.handle(record)
  File "/opt/smtx/python310/lib/python3.10/logging/__init__.py", line 966, in handle
    self.acquire()
  File "/root/zbs-client-py/deadlock/gevent_demo.py", line 17, in acquire
    self.zk.__del__()
  File "/root/zbs-client-py/src/zbs/lib/zk.py", line 86, in __del__
    self.stop()
  File "/root/zbs-client-py/src/zbs/lib/zk.py", line 91, in stop
    self.zk.stop()
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/client.py", line 688, in stop
    self._safe_close()
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/client.py", line 581, in _safe_close
    if not self._connection.stop(timeout):
  File "/usr/local/venv/zbs-client-py/lib/python3.10/site-packages/kazoo/protocol/connection.py", line 199, in stop
    self._connection_routine.join()
```
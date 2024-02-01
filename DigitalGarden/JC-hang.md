# 现场

 /var/log/zbs/job-center-worker.celery.INFO  日志内容

```
Sep  4 02:59:20 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  . tuna.job_center.scheduler.cron.sync_vsphere_accounts
Sep  4 02:59:20 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  . tuna.job_center.scheduler.cron.update_local_usbs
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: **Exception ignored in: <function Zookeeper.__del__ at 0x7f9715e1cc10>**
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Traceback (most recent call last):
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 87, in __del__
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:    self.stop()
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 97, in stop
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:    raise ZkException("stop zk client fail") from e
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: zbs.lib.zk.ZkException: stop zk client fail
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: **Exception ignored in: <function Zookeeper.__del__ at 0x7f9715e1cc10>**
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Traceback (most recent call last):
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 87, in __del__
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:    self.stop()
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 97, in stop
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:    raise ZkException("stop zk client fail") from e
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: zbs.lib.zk.ZkException: stop zk client fail
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Traceback (most recent call last):
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "greenify.pyx", line 70, in greenify.wait
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_waiter.py", line 143, in gevent._gevent_c_waiter.Waiter.get
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_waiter.py", line 154, in gevent._gevent_c_waiter.Waiter.get
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 61, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 64, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 67, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch_out
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 68, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch_out
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: gevent.exceptions.BlockingSwitchOutError: Impossible to call blocking function in the event loop callback
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Exception ignored in: 'greenify.wait_gevent'
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Traceback (most recent call last):
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "greenify.pyx", line 70, in greenify.wait
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_waiter.py", line 143, in gevent._gevent_c_waiter.Waiter.get
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_waiter.py", line 154, in gevent._gevent_c_waiter.Waiter.get
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 61, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 64, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 67, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch_out
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 68, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch_out
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: gevent.exceptions.BlockingSwitchOutError: Impossible to call blocking function in the event loop callback
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Traceback (most recent call last):
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "greenify.pyx", line 70, in greenify.wait
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_waiter.py", line 143, in gevent._gevent_c_waiter.Waiter.get
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_waiter.py", line 154, in gevent._gevent_c_waiter.Waiter.get
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 61, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 64, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 67, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch_out
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "src/gevent/_greenlet_primitives.py", line 68, in gevent._gevent_c_greenlet_primitives.SwitchOutGreenletWithLoop.switch_out
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: gevent.exceptions.BlockingSwitchOutError: Impossible to call blocking function in the event loop callback
```

可以看到的确有两个 greenlet （是 zk_loop）没有执行完成
```
[root@SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 13:16:43 ~]$ ps -ef | grep job-cen
root        3062       1  0 Aug30 ?        01:12:09 /usr/local/venv/job-center/bin/python3 /usr/bin/job-center scheduler --log-file /var/log/zbs/job-center-scheduler.INFO
root      631104  166504  0 13:16 pts/0    00:00:00 grep --color=auto job-cen
root     4076435       1  5 02:59 ?        00:34:19 /usr/local/venv/job-center/bin/python3 /usr/bin/job-center worker --log-file /var/log/zbs/job-center-worker.INFO

[root@SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 13:17:08 ~]$ grep kazoo -A 5 4076435.pystack.1 
  File "/usr/local/venv/job-center/lib/python3.10/site-packages/kazoo/handlers/gevent.py", line 78, in greenlet_worker
    func = queue.get()

---------------

  File "/usr/local/venv/job-center/lib/python3.10/site-packages/kazoo/handlers/gevent.py", line 78, in greenlet_worker
    func = queue.get()

---------------

  File "/opt/smtx/python310/lib/python3.10/threading.py", line 973, in _bootstrap
```

但是此时 依然这个worker并未卡死， 依然可以响应请求，卡死有其他原因, zk stop失败最多只会导致相应的event loop greenlet无法释放， 是一个比较次要的问题

```
[root@SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 13:37:36 ~]$ tail -f  /var/log/zbs/job-center-worker.INFO | grep suspend
[2023-09-04 13:38:33,440: INFO/MainProcess] Task smartx_app.elf.job_center.follower.kvm.vm.kvm_vm_suspend[9e2c8704-4181-4a1e-b73b-f20697181330] received
[2023-09-04 13:38:33,549: INFO/MainProcess] Task smartx_app.elf.job_center.follower.kvm.vm.kvm_vm_suspend[9e2c8704-4181-4a1e-b73b-f20697181330] succeeded in 0.10779126902343705s: None
```

## 一个没hang的例子
### celery 日志
```
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Exception ignored in: <function Zookeeper.__del__ at 0x7f9715e1cc10>
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Traceback (most recent call last):
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 87, in __del__
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:    self.stop()
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 97, in stop
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:    raise ZkException("stop zk client fail") from e
Sep  4 04:16:06 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: zbs.lib.zk.ZkException: stop zk client fail
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Exception ignored in: <function Zookeeper.__del__ at 0x7f9715e1cc10>
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Traceback (most recent call last):
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 87, in __del__
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:    self.stop()
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 97, in stop
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]:    raise ZkException("stop zk client fail") from e
Sep  4 10:26:04 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: zbs.lib.zk.ZkException: stop zk client fail
Sep  4 11:15:50 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[4076435]: Traceback (most recent call last):
```

### JC worker 日志
```
[2023-09-04 04:16:03,488: WARNING/MainProcess] Sync Monitor NFS export(dolphin-images) failed, error=
[2023-09-04 04:16:06,505: WARNING/MainProcess] Sync Monitor NFS export(dolphin-instances) failed, error=
[2023-09-04 04:16:06,993: INFO/MainProcess] Task smartx_app.elf.job_center.scheduler.cron.submit_recycle_stale_volumes[10256e55-f80f-4a04-a671-698d585bdd4e] received
[2023-09-04 04:16:07,009: INFO/MainProcess] Task smartx_app.elf.job_center.scheduler.cron.submit_recycle_stale_volumes[10256e55-f80f-4a04-a671-698d585bdd4e] succeeded in 0.015144068980589509s: None
[2023-09-04 04:16:10,036: INFO/MainProcess] Task job_center.handler.scheduler.worker.submit_worker_check_custom[344aa873-1654-4677-8b9a-c9526173e95d] received
[2023-09-04 04:16:10,044: INFO/MainProcess] Task smartx_app.elf.job_center.scheduler.cron.submit_clean_dirty_vm_schedule_results[6bec0340-6131-4e1b-bdfa-22c324f0a399] received
```


## 一个hang住的例子
### celery 日志
```
Sep  4 02:56:41 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[3878359]: Exception ignored in: <function Zookeeper.__del__ at 0x7f69e3bb0c10>
Sep  4 02:56:41 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[3878359]: Traceback (most recent call last):
Sep  4 02:56:41 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[3878359]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 87, in __del__
Sep  4 02:56:41 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[3878359]:    self.stop()
Sep  4 02:56:41 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[3878359]:  File "/usr/local/venv/job-center/lib/python3.10/site-packages/zbs/lib/zk.py", line 97, in stop
Sep  4 02:56:41 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[3878359]:    raise ZkException("stop zk client fail") from e
Sep  4 02:56:41 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[3878359]: zbs.lib.zk.ZkException: stop zk client fail
Sep  4 02:59:01 SMTXELF-6-0-0-B23SmokeAPIX20230830075255X3 job-center[3878359]: worker: Warm shutdown (MainProcess)
```
### JC worker 日志
```
[2023-09-04 02:56:41,096: INFO/MainProcess] Task job_center.handler.scheduler.worker.worker_state_check_disconnect[4f1c4617-d951-4245-a9c5-67bc9c8d8aed] succeeded in 0.05183629802195355s: None
[2023-09-04 02:56:41,102: INFO/MainProcess] Task job_center.handler.scheduler.worker.worker_state_check_custom[e43d45ba-f9ff-445f-b033-130cc705cf4b] succeeded in 0.06318917497992516s: None
[2023-09-04 02:56:41,128: INFO/MainProcess] Task smartx_app.elf.job_center.scheduler.vm.kvm_vm_check[5a5678de-bef3-49c3-a7fb-2b8992241071] succeeded in 0.09658635401865467s: None
[2023-09-04 02:59:13,452: INFO/MainProcess] Is /usr/lib64/libvirt.so.0 patched by greenify? True
[2023-09-04 02:59:13,469: INFO/MainProcess] Is /usr/lib64/libglib-2.0.so.0 patched by greenify? True
[2023-09-04 02:59:16,848: INFO/MainProcess] Collect node info with SR-IOV Nic infos when job-center-worker start
```
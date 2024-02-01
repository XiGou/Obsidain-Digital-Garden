# C Examples

# Python Examples


## x86 机器
```
$ /usr/local/venv/zbs-client-py/bin/python /usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/examples/inc_backup_fine.py --zbs-ip 127.0.0.1
src_volume_id:  7930c9a7-96bc-462b-b0bb-be4bedea60d1  target_volume_id:  933d734f-e34b-43f3-8eab-c3243ea74c9a
test env setup done
..........
full backup total_data_count: 2621440
compare 7c87cd63-8a4f-4757-b334-3f9c90049ef8 with 933d734f-e34b-43f3-8eab-c3243ea74c9a started
compare volume 933d734f-e34b-43f3-8eab-c3243ea74c9a with snapshot 7c87cd63-8a4f-4757-b334-3f9c90049ef8 completed successfully
.
inc backup total_data_count: 262144
compare a5e6c6df-ea95-4b86-8721-683a893b685c with 933d734f-e34b-43f3-8eab-c3243ea74c9a started
compare volume 933d734f-e34b-43f3-8eab-c3243ea74c9a with snapshot a5e6c6df-ea95-4b86-8721-683a893b685c completed successfully
$ /usr/local/venv/zbs-client-py/bin/python /usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/examples/inc_backup_by_extent.py --zbs-ip 127.0.0.1
test-src-volume test-target-volume already existed, delete them
src_volume_id:  87b1e167-ee1c-4e61-b97e-7079a2e4831e  target_volume_id:  7c7aeef3-b524-4410-b6db-d8e9e920fbd5
==============full backup, target should be same as snap-1 when finished===============
backing-up extent 14889
backing-up extent 14882
backing-up extent 14883
backing-up extent 14884
check backup done
full backup done
==============incremental backup, target should be same as snap-2 when finished===============
backing-up extent 14889
skip not changed extent 14882
skip not changed extent 14883
skip not changed extent 14884
check backup done
inc backup done
```


## arm 机器会core

```
 $rpm -qa | grep zbs
libzbs-5.4.3-rc7.0.release.git.ga6891abbd.oe1.SMTX.HCI.aarch64
zbs-perf-tools-1.0.1-rc5.0.g3a15ca0.oe1.aarch64
zbs-debuginfo-5.4.3-rc7.0.release.git.ga6891abbd.oe1.SMTX.HCI.aarch64
pyzbs-6.0.0-rc11.0.git.g79db66259.oe1.aarch64
zbs-5.4.3-rc7.0.release.git.ga6891abbd.oe1.SMTX.HCI.aarch64
zbs-client-py3-5.4.2-rc10.0.oe1.aarch64
```

```
$ /usr/local/venv/zbs-client-py/bin/python /usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/examples/inc_backup_by_extent.py --zbs-ip 127.0.0.1
test-src-volume test-target-volume already existed, delete them
src_volume_id:  c103f92f-42d1-44c4-be8d-b242ec733cd9  target_volume_id:  2a46aedd-eae8-4aa3-aa53-07801a8713c5
==============full backup, target should be same as snap-1 when finished===============
backing-up extent 1250
backing-up extent 1243
backing-up extent 1244
backing-up extent 1245
check backup done
full backup done
==============incremental backup, target should be same as snap-2 when finished===============
backing-up extent 1250
skip not changed extent 1243
skip not changed extent 1244
skip not changed extent 1245
check backup done
inc backup done
```

```
$ /usr/local/venv/zbs-client-py/bin/python /usr/local/venv/zbs-client-py/lib/python3.10/site-packages/zbs/examples/inc_backup_fine.py --zbs-ip 127.0.0.1
src_volume_id:  cbc5b03a-68cc-481e-98b1-bfa7cef19b54  target_volume_id:  6ee3e7d1-8f33-4d2c-8095-b9a3bb239162
test env setup done
Segmentation fault (core dumped)
```

```
Program terminated with signal SIGSEGV, Segmentation fault.
#0  zbs::libzbs::SubTask::Release (this=0x3010000000000) at src/libzbs/compare_volume_manager.cc:312
312     src/libzbs/compare_volume_manager.cc: No such file or directory.
[Current thread is 1 (Thread 0xfffeea0af1e0 (LWP 2566953))]
(gdb) bt
#0  zbs::libzbs::SubTask::Release (this=0x3010000000000) at src/libzbs/compare_volume_manager.cc:312
#1  zbs::libzbs::CompareVolumeManager::CompareVolumeTask::~CompareVolumeTask (this=0xfffee988fca0, __in_chrg=<optimized out>) at src/libzbs/compare_volume_manager.cc:426
#2  0x0000fffeec80100c in zbs::libzbs::CompareVolumeManager::CompareVolumeWithBlank (this=0xfffee0042810, volume_id=..., start_gb=<optimized out>, length_gb=<optimized out>, stripe_size=<optimized out>, 
    stripe_num=<optimized out>, result=0xfffeeca9c8b0) at src/libzbs/compare_volume_manager.cc:923
#3  0x0000fffeec83edbc in zbs::libzbs::ZbsClientProxy::<lambda()>::operator() (__closure=0xfffee084f6f8) at src/libzbs/zbs_client_proxy.cc:348
#4  zbs::CoroutineClosure<zbs::libzbs::ZbsClientProxy::CompareVolume(const string&, const string&, uint32_t, uint32_t, uint32_t, uint32_t, std::vector<zbs::RangeU64>*)::<lambda()> >::<lambda()>::operator() (
    __closure=0xfffee00054f0) at src/common/coroutine_utils.h:94
#5  zbs::CoFunc::Impl<zbs::CoroutineClosure<Func>::Run() [with Func = zbs::libzbs::ZbsClientProxy::CompareVolume(const string&, const string&, uint32_t, uint32_t, uint32_t, uint32_t, std::vector<zbs::RangeU64>*)::<lambda()>]::<lambda()> >::s_run_and_reset(zbs::CoFunc::storage_t &) (storage=...) at src/common/coroutine.h:276
#6  0x0000fffeec846a3c in zbs::CoFunc::run_and_reset (this=0xfffee00054e0) at src/common/coroutine.h:312
#7  zbs::Coroutine::LoopMain (this=0xfffee0005360) at src/common/coroutine.cc:615
#8  0x0000fffeec846aa4 in zbs::Coroutine::s_co_main (lo=<optimized out>, hi=<optimized out>) at src/common/coroutine.cc:602
```

# Go Examples
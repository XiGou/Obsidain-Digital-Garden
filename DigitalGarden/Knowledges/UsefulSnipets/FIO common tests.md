---
dg-publish: true
Last Edited: 2023-11-17T23:58
date: 2022-11-05 00:22:33
---

```bash
fio --name=random_rw --ioengine=libaio --rw=randrw --bs=4k --numjobs=1 --size=0 --time_based=1 --runtime=-1 --rate=10M --filename=/dev/target_disk
```

### fsync test

```bash
 fio --rw=randwrite --ioengine=sync --fdatasync=1 --directory=test-data --size=200m --bs=256K --name=mytest
```

### rated background IO

```bash
 fio --rw=write --ioengine=libaio   --filename=/dev/vdb --bs=4k --iodepth=128 --name=mytest --rate=50M --direct=1
```

[[etcd deploy and test]]
##  Client-Server Mode
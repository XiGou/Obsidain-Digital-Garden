---
dg-publish: true
---

```bash
fio --name=random_rw --ioengine=libaio --rw=randrw --bs=4k --numjobs=1 --size=0 --time_based=1 --runtime=-1 --rate=10M --filename=/dev/target_disk
```

##  Client-Server Mode
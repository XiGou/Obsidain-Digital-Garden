---
dg-publish: true
---

# Gen Random data
```bash
dd if=/dev/urandom of=random_data.bin bs=1M count=1024
```

# Enable Coredump
```bash
ulimit -c unlimited
echo "core.%e.%p" > /proc/sys/kernel/core_pattern
```
# 查看空间占用
- ncdu
- du
# Watch the specified file and sync it to test enviroment when changed.
```Shell
while true; do inotifywait file.py; scp file.py root@x.x.x.x:/root; done;
```
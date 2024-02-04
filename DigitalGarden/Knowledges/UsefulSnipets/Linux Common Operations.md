---
dg-publish: true
Last Edited: 2023-11-17T23:58
date: 2022-11-05 00:22:33
---

## Gen Random data
Also used to test IO performance.

```bash
dd if=/dev/urandom of=random_data.bin bs=1M count=1024
```

## Enable Coredump

```bash
ulimit -c unlimited
echo "core.%e.%p" > /proc/sys/kernel/core_pattern
```

## Check Disk Space Usage
- ncdu
- du
## Watch the specified file and sync it to test enviroment when changed.

```Shell
while true; do inotifywait file.py; scp file.py root@x.x.x.x:/root; done;
```

## Network
- ip
- netstat
- nload: show realtime network load graph in terminal

## rsync

Sync Source Directory to Destination Directory

```bash
rsync -av --delete ./src_path root@x.x.x.x:/dst_path
```
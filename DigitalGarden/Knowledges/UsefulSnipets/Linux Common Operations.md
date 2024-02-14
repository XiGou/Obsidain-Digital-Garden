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
[[File Transfering#rsync]]

```Shell

while true; do inotifywait file.py; scp file.py root@x.x.x.x:/root; done;


while true; do inotifywait -r /path/to/src; rsync -av --delete /src /dst; done;
```

## Network
- ip
- netstat
- nload: show realtime network load graph in terminal

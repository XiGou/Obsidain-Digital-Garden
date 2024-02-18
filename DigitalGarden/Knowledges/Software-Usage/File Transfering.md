---
date: 2024-01-06 01:14:31 +0800
title: "File Transfering"
---

## rsync

^83e05d

[rsync 用法教程 - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2020/08/rsync.html)

sync source to destination 

```bash
# result to: destination/source
rsync -r source destination
# result to: destination/file_in_source
rsync -r source/ destination  
# result: with metadata
rsync -a source destination
# -n dry run, -v verbose
rsync -anv source/ destination
# make src and dst totally same, --delete will delete file in dst but not in src.
rsync -av --delete source/ destination
# ssh protocal
rsync -av --delete source/ username@remote_host:destination
```
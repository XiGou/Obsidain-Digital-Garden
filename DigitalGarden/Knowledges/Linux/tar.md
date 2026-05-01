---
tags:
  - bash
  - cmd_usage
---

# tar 常用命令笔记

> `tar` = **T**ape **A**rchive（磁带归档） 选项可以不加 `-`，直接写字母组合。

---

## 核心选项助记

|选项|全称|含义|
|---|---|---|
|`c`|**C**reate|创建归档|
|`x`|e**X**tract|解压/提取|
|`t`|lis**T**|列出内容（不解压）|
|`f`|**F**ile|指定归档文件名（几乎必须加）|
|`v`|**V**erbose|显示详细过程|
|`z`|g**Z**ip|用 gzip 压缩/解压（`.tar.gz`）|
|`j`|bzip**2**|用 bzip2 压缩/解压（`.tar.bz2`）|
|`J`|x**Z**|用 xz 压缩/解压（`.tar.xz`）|
|`C`|**C**hange directory|指定解压目标目录|
|`p`|**P**ermissions|保留文件权限|
|`--exclude`|—|排除某些文件/目录|

---

## 常用场景

### 查看归档内容（不解压）

```bash
tar -tf archive.tar.gz
```

> `t` = lisT（列出） `f` = File（指定文件）

加 `v` 显示详细信息（权限、大小、时间）：

```bash
tar -tvf archive.tar.gz
```

---

### 解压

```bash
# 解压到当前目录
tar -xzf archive.tar.gz

# 解压到指定目录
tar -xzf archive.tar.gz -C /opt/app/

# 解压 .tar.bz2
tar -xjf archive.tar.bz2

# 解压 .tar.xz
tar -xJf archive.tar.xz

# 解压时显示进度
tar -xzvf archive.tar.gz
```

> `x` = eXtract `z` = gZip `f` = File `C` = Change dir

---

### 创建归档

```bash
# 创建 .tar.gz
tar -czf output.tar.gz /path/to/dir

# 创建 .tar.bz2（压缩率更高，更慢）
tar -cjf output.tar.bz2 /path/to/dir

# 创建 .tar.xz（压缩率最高，最慢）
tar -cJf output.tar.xz /path/to/dir

# 打包时显示进度
tar -czvf output.tar.gz /path/to/dir
```

> `c` = Create `z` = gZip `f` = File（必须紧跟文件名）

---

### 排除文件/目录

```bash
tar -czf backup.tar.gz /var/app \
    --exclude='*.log' \
    --exclude='/var/app/tmp'
```

---

### 只提取特定文件

```bash
# 只提取某个文件
tar -xzf archive.tar.gz path/inside/archive/file.conf

# 只提取某个目录
tar -xzf archive.tar.gz path/inside/archive/subdir/
```

---

### 追加文件到归档（仅限未压缩 .tar）

```bash
tar -rf archive.tar newfile.txt
```

> `r` = append（追加），不支持 `.tar.gz`，需先解压再打包

---

## 快速记忆口诀

```
创建  c Create
解压  x eXtract
列出  t lisT
文件  f File
压缩  z gZip
详情  v Verbose
目录  C Change dir
```

**组合套路：**

|目的|命令模板|
|---|---|
|看内容|`tar -tf xxx.tar.gz`|
|解压|`tar -xzf xxx.tar.gz`|
|打包|`tar -czf out.tar.gz /path`|
|解压到指定目录|`tar -xzf xxx.tar.gz -C /target/`|

---

## 后缀与压缩格式对应

|后缀|选项|压缩程序|
|---|---|---|
|`.tar`|无|仅打包，不压缩|
|`.tar.gz` / `.tgz`|`z`|gzip（速度快，最常用）|
|`.tar.bz2`|`j`|bzip2（压缩率高）|
|`.tar.xz`|`J`|xz（压缩率最高）|

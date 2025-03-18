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
# Multi-Thread
rsync -av -P --delete source/ username@remote_host:destination
```

## axel

```
# download file with 10 connections
axel -n 10 url -o file
```

## curl

[Curl cheatsheet](https://devhints.io/curl)

### Options

```bash
-o <file> # --output: write to file -u user:pass # --user: Authentication
```

```bash
-v # --verbose -vv # Even more verbose -s # --silent: don't show progress meter or errors -S # --show-error: when used with --silent (-sS), show errors but no progress meter
```

```bash
-i # --include: Include the HTTP-header in the output -I # --head: headers only
```

### Request

```bash
-X POST # --request -L # follow link if page redirects -F # --form: HTTP POST data for multipart/form-data
```

### Data

```bash
-d 'data' # --data: HTTP post data, URL encoded (eg, status="Hello") -d @file # --data via file -G # --get: send -d data via get
```

```bash
-A <str> # --user-agent -b name=val # --cookie -b FILE # --cookie -H "X-Foo: y" # --header --compressed # use deflate/gzip
```

### SSL

```bash
--cacert <file> --capath <dir>
```

```bash
-E, --cert <cert> # --cert: Client cert file --cert-type # der/pem/eng -k, --insecure # for self-signed certs
```

### Examples

```bash
# Post data: curl -d password=x http://x.com/y
```

```bash
# Auth/data: curl -u user:pass -d status="Hello" http://twitter.com/statuses/update.xml
```

```bash
# multipart file upload curl -v --include --form key1=value1 --form upload=@localfilename URL # multipart form: send data from text field and upload file curl -F person=anonymous -F secret=@file.txt http://example.com/submit.cgi
```

```bash
# Use Curl to Check if a remote resource is available # details: https://matthewsetter.com/check-if-file-is-available-with-curl/ curl -o /dev/null --silent -Iw "%{http_code}" https://example.com/my.remote.tarball.gz
```
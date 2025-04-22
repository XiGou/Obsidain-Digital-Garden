

## API

Upload a file

```shell
curl -T path-to-file http://127.0.0.1:5000/new-path/path-to-file
```

Upload a folder

```bash
#!/bin/bash

# 检查参数
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <local_directory> <dufs_url>"
    exit 1
fi

LOCAL_DIR="$1"
DUFS_URL="$2"

# 确保目录存在
if [ ! -d "$LOCAL_DIR" ]; then
    echo "Error: Directory '$LOCAL_DIR' does not exist."
    exit 1
fi

# 遍历目录并上传文件
find "$LOCAL_DIR" -type f | while read -r file; do
    relative_path="${file#$LOCAL_DIR/}"  # 获取相对路径
    target_url="${DUFS_URL%/}/$relative_path"  # 拼接目标 URL

    echo "Uploading '$file' to '$target_url'..."
    curl -T "$file" "$target_url"

    if [ $? -ne 0 ]; then
        echo "Failed to upload '$file'."
    fi
done

```

Download a file

```shell
curl http://127.0.0.1:5000/path-to-file           # download the file
curl http://127.0.0.1:5000/path-to-file?hash      # retrieve the sha256 hash of the file
```

Download a folder as zip file

```shell
curl -o path-to-folder.zip http://127.0.0.1:5000/path-to-folder?zip
```

Delete a file/folder

```shell
curl -X DELETE http://127.0.0.1:5000/path-to-file-or-folder
```

Create a directory

```shell
curl -X MKCOL http://127.0.0.1:5000/path-to-folder
```

Move the file/folder to the new path

```shell
curl -X MOVE http://127.0.0.1:5000/path -H "Destination: http://127.0.0.1:5000/new-path"
```

List/search directory contents

```shell
curl http://127.0.0.1:5000?q=Dockerfile           # search for files, similar to `find -name Dockerfile`
curl http://127.0.0.1:5000?simple                 # output names only, similar to `ls -1`
curl http://127.0.0.1:5000?json                   # output paths in json format
```

With authorization (Both basic or digest auth works)

```shell
curl http://127.0.0.1:5000/file --user user:pass                 # basic auth
curl http://127.0.0.1:5000/file --user user:pass --digest        # digest auth
```

Resumable downloads

```shell
curl -C- -o file http://127.0.0.1:5000/file
```

Resumable uploads

```shell
upload_offset=$(curl -I -s http://127.0.0.1:5000/file | tr -d '\r' | sed -n 's/content-length: //p')
dd skip=$upload_offset if=file status=none ibs=1 | \
  curl -X PATCH -H "X-Update-Range: append" --data-binary @- http://127.0.0.1:5000/file
```

Health checks

```shell
curl http://127.0.0.1:5000/__dufs__/health
```

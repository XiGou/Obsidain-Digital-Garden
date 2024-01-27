---
dg-publish: true
---
## 安装方式

## 常用操作
### 压缩一个文件夹下面的所有图片
```bash
mkdir backup
for file in *.png; do ffmpeg -i "$file" -compression_level 100 -y "backup/${file}_compressed.png"; done

```
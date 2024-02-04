---
dg-publish: true
title: ffmpeg
date: 2024-01-06 01:14:31 +0800
---

## Common Use Case
### Compress all images under a directory

```bash
mkdir backup
for file in *.png; do ffmpeg -i "$file" -compression_level 100 -y "backup/${file}_compressed.png"; done

```
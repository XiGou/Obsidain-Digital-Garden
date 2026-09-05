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


# 🎵 **音频处理（Audio）**

## ⭐ **1. 提取音频（从视频抽出 MP3 / WAV）**
```bash
ffmpeg -i input.mp4 -vn -acodec mp3 output.mp3
```

## ⭐ **2. 音频格式转换（MP3 ↔ WAV）**
```bash
ffmpeg -i input.wav output.mp3
```

## ⭐ **3. 音量增益（放大音量）**
```bash
ffmpeg -i input.mp3 -filter:a "volume=2.0" output.mp3
```
或 dB：
```bash
ffmpeg -i input.mp3 -filter:a "volume=8dB" output.mp3
```

## ⭐ **4. 裁剪音频（从 0 到 113 秒）**
```bash
ffmpeg -i input.mp3 -t 113 output.mp3
```

从 113 秒开始：
```bash
ffmpeg -i input.mp3 -ss 113 output.mp3
```

## ⭐ **5. 合并多个音频（拼接）**
文件列表：
```
file a.mp3
file b.mp3
file c.mp3
```

命令：
```bash
ffmpeg -f concat -safe 0 -i list.txt -c copy output.mp3
```

## ⭐ **6. 混音（叠加两段音频）**
```bash
ffmpeg -i a.mp3 -i b.mp3 -filter_complex amix=inputs=2:duration=longest output.mp3
```

---

# 🎬 **视频处理（Video）**

## ⭐ **1. 裁剪视频（按时间）**
```bash
ffmpeg -i input.mp4 -ss 00:00:10 -t 20 output.mp4
```
从 10 秒开始，截取 20 秒。

## ⭐ **2. 裁剪画面（按区域）**
```bash
ffmpeg -i input.mp4 -filter:v "crop=1280:720:0:0" output.mp4
```

## ⭐ **3. 压缩视频（最常用）**
```bash
ffmpeg -i input.mp4 -vcodec libx264 -crf 23 output.mp4
```
CRF 越大越小（23 是常用值）。

## ⭐ **4. 转码（H.265）**
```bash
ffmpeg -i input.mp4 -vcodec libx265 -crf 28 output.mp4
```

## ⭐ **5. 提取视频帧（截图）**
```bash
ffmpeg -i input.mp4 -ss 10 -vframes 1 output.png
```

## ⭐ **6. 导出所有帧**
```bash
ffmpeg -i input.mp4 frame_%04d.png
```

---

# 🎧 **音视频混合（Audio + Video）**

## ⭐ **1. 替换视频音轨**
```bash
ffmpeg -i video.mp4 -i audio.mp3 -c:v copy -map 0:v:0 -map 1:a:0 output.mp4
```

## ⭐ **2. 给视频加背景音乐（混合）**
```bash
ffmpeg -i video.mp4 -i bgm.mp3 -filter_complex amix=inputs=2 output.mp4
```

## ⭐ **3. 去掉视频音频**
```bash
ffmpeg -i input.mp4 -an output.mp4
```

---

# 🏷 **元数据（Metadata）**

## ⭐ **1. 给音频加封面图**
```bash
ffmpeg -i input.mp3 -i cover.jpg -map 0 -map 1 -c copy -id3v2_version 3 output.mp3
```

## ⭐ **2. 修改标题/作者**
```bash
ffmpeg -i input.mp3 -metadata title="My Title" -metadata artist="Xi" output.mp3
```

---

# 📦 **批量处理（Shell）**

## ⭐ **1. 批量转换 MP3 → WAV**
```bash
for f in *.mp3; do ffmpeg -i "$f" "${f%.mp3}.wav"; done
```

## ⭐ **2. 批量裁剪到 113 秒**
```bash
for f in *.mp3; do ffmpeg -i "$f" -t 113 "cut_$f"; done
```

---

# 🧠 **工程师常用 ffmpeg 参数速查表**

| 参数 | 作用 |
|------|------|
| `-i` | 输入文件 |
| `-ss` | 起始时间 |
| `-t` | 截取时长 |
| `-vn` | 去掉视频 |
| `-an` | 去掉音频 |
| `-c copy` | 不重新编码 |
| `-filter:a` | 音频滤镜 |
| `-filter:v` | 视频滤镜 |
| `-map` | 选择音轨/视频轨 |
| `-crf` | 视频质量控制 |

---

# 🟦 想继续扩展吗？
我可以帮你继续整理：

- **音频工程师常用 ffmpeg 命令合集**  
- **视频剪辑常用 ffmpeg 工作流**  
- **为你生成一个完整的 ffmpeg 速查手册 PDF 结构**  

你想继续扩展哪一部分？
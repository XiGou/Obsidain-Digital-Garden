# yt-dlp 命令速查表

> 注：`youtube-dl` 已停止维护，`yt-dlp` 是社区维护的功能更全的分支，命令参数基本兼容并做了大量扩展，推荐直接使用 `yt-dlp`。

## 安装

```bash
# pip 安装（推荐，方便升级）
pip install -U yt-dlp

# 或直接下载二进制（Linux）
sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp

# 升级
yt-dlp -U
```

---

## 基础用法

```bash
# 最简单下载（默认选择最佳画质/音质合并）
yt-dlp "URL"

# 只看信息，不下载
yt-dlp --dump-json "URL"
yt-dlp -F "URL"          # 列出所有可用格式（format code）
```

---

## 格式选择 `-f`

```bash
yt-dlp -f best "URL"                  # 单文件最佳质量（可能画质有上限）
yt-dlp -f bestvideo+bestaudio "URL"    # 最佳视频+最佳音频，自动合并（需要 ffmpeg）
yt-dlp -f "bv*+ba/b" "URL"             # 常用写法，无最佳组合时回退到单文件最佳
yt-dlp -f 137+140 "URL"                # 指定具体 format code（配合 -F 查看）

# 限制分辨率/编码
yt-dlp -f "bv*[height<=1080]+ba" "URL"
yt-dlp -f "bv*[vcodec^=avc]+ba" "URL"  # 优先 h264 编码，兼容性更好

# 只要音频
yt-dlp -f bestaudio "URL"
yt-dlp -x --audio-format mp3 "URL"     # 提取并转码为 mp3（需要 ffmpeg）
yt-dlp -x --audio-format mp3 --audio-quality 0 "URL"  # 0 = 最佳质量
```

---

## 输出与文件命名 `-o`

```bash
yt-dlp -o "%(title)s.%(ext)s" "URL"
yt-dlp -o "%(uploader)s/%(upload_date)s - %(title)s.%(ext)s" "URL"

# 常用字段：%(id)s %(title)s %(ext)s %(uploader)s %(upload_date)s
#          %(duration)s %(resolution)s %(playlist_index)s
```

---

## 播放列表 / 批量

```bash
yt-dlp "PLAYLIST_URL"                       # 下载整个播放列表
yt-dlp --playlist-items 1,3,5-10 "URL"      # 只下载指定序号
yt-dlp --no-playlist "URL"                  # 单个链接属于播放列表时，只下载该视频
yt-dlp -a urls.txt                          # 从文本文件批量读取 URL（每行一个）
```

---

## 字幕

```bash
yt-dlp --write-sub --sub-lang zh-Hans,en "URL"   # 下载指定语言字幕（不下载视频加 --skip-download）
yt-dlp --write-auto-sub --sub-lang zh-Hans "URL" # 下载自动生成字幕
yt-dlp --embed-subs "URL"                        # 字幕嵌入视频文件
yt-dlp --list-subs "URL"                         # 查看可用字幕语言
```

---

## 断点续传 / 限速 / 重试

```bash
yt-dlp -c "URL"                       # 断点续传（continue，默认开启）
yt-dlp -r 2M "URL"                    # 限速 2MB/s
yt-dlp --retries 10 "URL"             # 失败重试次数
yt-dlp --concurrent-fragments 4 "URL" # 分片并发下载数（提速）
```

---

## Cookies / 登录内容

```bash
yt-dlp --cookies cookies.txt "URL"                 # 使用导出的 cookies 文件
yt-dlp --cookies-from-browser chrome "URL"         # 直接读取浏览器 cookies（chrome/firefox/edge...）
```

---

## 元数据 / 缩略图 / 附加信息

```bash
yt-dlp --embed-thumbnail "URL"        # 嵌入封面图
yt-dlp --embed-metadata "URL"         # 嵌入标题、作者等元数据
yt-dlp --write-thumbnail "URL"        # 单独保存缩略图文件
yt-dlp --write-info-json "URL"        # 保存完整元数据为 json
yt-dlp --write-description "URL"      # 保存视频简介为 txt
```

---

## 时间区间下载

```bash
# 只下载视频的某一段（需要 ffmpeg，精确剪辑）
yt-dlp --download-sections "*00:01:00-00:02:30" "URL"

# 下载多段
yt-dlp --download-sections "*00:00:00-00:01:00" --download-sections "*00:05:00-00:06:00" "URL"
```

---

## 直播 / 特殊场景

```bash
yt-dlp --live-from-start "URL"        # 从直播开始处录制（若支持）
yt-dlp --wait-for-video 30 "URL"      # 等待预告直播开始
```

---

## 代理与网络

```bash
yt-dlp --proxy "socks5://127.0.0.1:1080" "URL"
yt-dlp --socket-timeout 30 "URL"
```

---

## 常用组合示例

```bash
# 下载 1080p 及以下 h264 编码 mp4，嵌入字幕和封面，按上传者归档
yt-dlp -f "bv*[height<=1080][vcodec^=avc]+ba" \
  --merge-output-format mp4 \
  --embed-subs --sub-lang zh-Hans,en \
  --embed-thumbnail --embed-metadata \
  -o "%(uploader)s/%(title)s.%(ext)s" \
  "URL"

# 只提取音频为高质量 mp3，附带封面和元数据
yt-dlp -x --audio-format mp3 --audio-quality 0 \
  --embed-thumbnail --add-metadata \
  -o "%(title)s.%(ext)s" \
  "URL"
```

---

## 依赖提示

- 合并音视频、剪辑片段、嵌入字幕/封面等功能都依赖 **ffmpeg**，请确保已安装并在 PATH 中。
- 遇到解析失败时优先尝试 `yt-dlp -U` 升级到最新版，很多站点改版后需要更新支持。
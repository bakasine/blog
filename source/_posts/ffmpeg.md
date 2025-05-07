---
title: ffmpeg 媒体处理简单操作
date: 2025-05-08 19:19:39
tags:
    - ffmpeg
categories: 
    - note
---

# <h2 id="cut">剪切</h2>

```bash
ffmpeg -i 原文件 -ss 开始时间 -to 结束时间 -c copy 输出名称
```

# <h2 id="audio">提取音频</h2>

```bash
ffmpeg -i mp4文件 -q:a 0 -map a 输出mp3文件
```

# <h2 id="muti_cut">剪切多个片段合并</h2>

```bash
ffmpeg -i 视频原文件 \
-filter_complex \
"[0:v]trim=start=片段1开始秒数:end=片段1结束秒数,setpts=PTS-STARTPTS[v1]; \
 [0:a]atrim=start=片段1开始秒数:end=片段1结束秒数,asetpts=PTS-STARTPTS[a1]; \
 [0:v]trim=start=片段2开始秒数:end=片段2结束秒数,setpts=PTS-STARTPTS[v2]; \
 [0:a]atrim=start=片段2开始秒数:end=片段2结束秒数,asetpts=PTS-STARTPTS[a2]; \
 [0:v]trim=start=片段3开始秒数:end=片段3结束秒数,setpts=PTS-STARTPTS[v3]; \
 [0:a]atrim=start=片段3开始秒数:end=片段3结束秒数,asetpts=PTS-STARTPTS[a3]; \
 [v1][a1][v2][a2][v3][a3]concat=n=3:v=1:a=1[outv][outa]" \
-map "[outv]" -map "[outa]" -c:v libx264 -crf 23 -c:a aac 输出视频文件
```

# <h2 id="muti_cut_audio">剪切多个片段合并转换成音频</h2>

```bash
ffmpeg -i '输入源文件' \
-filter_complex \
"[0:a]atrim=start=片段1开始秒数:end=片段1结束秒数,asetpts=PTS-STARTPTS[a1]; \
 [0:a]atrim=start=片段2开始秒数:end=片段2结束秒数,asetpts=PTS-STARTPTS[a2]; \
 [0:a]atrim=start=片段3开始秒数:end=片段3结束秒数,asetpts=PTS-STARTPTS[a3]; \
 [a1][a2][a3]concat=n=3:v=0:a=1[outa]" \
-map "[outa]" -c:a libmp3lame -q:a 0 \
'输出mp3名'
```
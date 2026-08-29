---
title: yt-dlp
date: 2026-08-29 16:16:49
tags:
---

基础：
`yt-dlp --cookies-from-browser firefox "https://www.youtube.com/watch?v="`
`yt-dlp --cookies-from-browser chromium "https://www.bilibili.com/video/"`

视频下载：
`-F`查看规格列表
`--list-subs`查看字幕（包括弹幕）列表
`--skip-download --write-subs --sub-format srt --convert-subs srt --sub-langs zh`只下载字幕
`-t mkv` 封装
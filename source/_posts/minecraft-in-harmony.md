---
title: 在 Harmony 6 的卓易通上使用 FCL 启动 MC 避坑记录
date: 2025-12-06 03:04:26
tags:
---

最近尝试在原生鸿蒙上启动 MC，但是只有使用 JRE8 的 1.12 及以下版本可以启动。

因此猜测是JRE的问题。Harmony 6 上的卓易通里的 JRE 貌似有些不完整，缺少一些库。

**解决办法使用[aaaapai/android-openjdk-build](https://github.com/aaaapai/android-openjdk-build)这个第三方修改版jre。**

大部分高版本的模组都只支持 Java 21，但是这个仓库里只提供了 JRE25 的构建成品，JRE21 的构建成品因为过了太久而被 Github 删掉了。因此作者重新构建了一份 JRE21 ，放在了 我克隆的仓库[releases](https://github.com/Ginsway/android-openjdk-build/releases)里，有需要的可以直接下载。


---

另外有2个小贴士：

1.在使用卓易通自带的文件互传时，在容器内部对应的目录叫做 `ShareFile/`  
2.导入的文件应该是后缀为 `tar.xz` 的压缩包
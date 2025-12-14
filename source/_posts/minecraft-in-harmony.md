---
title: 在 Harmony 6 的卓易通上使用 FCL 启动 MC 1.21 Neoforge 避坑记录
date: 2025-12-14 11:57:26
tags:
---

最近尝试在原生鸿蒙上启动 MC，但是只有使用 JRE8 的 1.12 及以下版本可以启动。

因此猜测是JRE的问题。FCL 里的 JRE 貌似有些对 arm 支持不好，缺少一些库。

**解决办法使用[aaaapai/android-openjdk-build](https://github.com/aaaapai/android-openjdk-build)这个第三方修改版jre。**

如果你使用的一些模组无法使用 Java 25 运行。可以尝试更新模组，如果再不行可以试试用 Java 21。
但是这个仓库里只提供了 JRE25 的构建成品，JRE21 的构建成品因为过了太久而被 Github 删掉了。因此重新构建了一份 JRE21 ，放在了 我克隆的仓库[releases](https://github.com/Ginsway/android-openjdk-build/releases)里，有需要的可以直接下载。


---

另外有2个小贴士：

1.在使用卓易通自带的文件互传时，在容器内部对应的目录叫做 `ShareFile/`  
2.导入的文件应该是后缀为 `tar.xz` 的压缩包
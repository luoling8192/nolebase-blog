---
title: 黑苹果关闭SIP导致Steam闪退
date: 2023-10-21 02:10:19
tags:
    - macOS
    - SIP
    - Hackintosh
    - Steam
    - Postman
    - 百度网盘
---

# 黑苹果关闭 SIP 导致 Steam 闪退

## 起因
在`OpenCore`中把 SIP 关了，本来想获得性能的提升，但是 Steam、Postman、百度网盘等一系列软件 打开以后闪退，调试信息显示显卡相关的问题。

Steam 会先启动更新器，更新完成后，登录窗口打不开，而 Postman 是直接闪退，而且没有崩溃提示。

## 解决
再把 SIP 完全打开就好了。我也不知道是什么原因，可能和 Electron 有关。

---
title: Windows下Webstorm无法使用安装的字体
date: 2024-03-12 22:21:00
tags:
    - Windows
    - Jetbrains
    - Webstorm
---

# Windows 下 Webstorm 无法使用安装的字体

## 起因
很长一段时间，我的 Webstorm 是无法使用我自行安装的字体的，因为修改编辑器字体那里根本就找不到。但是如果直接编辑 Webstorm 的配置文件，虽然设置那里会提示找不到该字体，但是可以正常使用。不过这样有一个坏处：无法获得合适的字重。

<!--more-->

## 发现
在有一次偶然的发现中，我找到了导致这个奇葩问题的原因。

在 Windows 中除了有系统的字体文件夹（`C:\Windows\Fonts\`）以外，还存在着用户自己的字体文件夹，这也是导致 Webstorm 找不到字体的罪魁祸首。

## 解决
用户字体文件夹在 `%LocalAppData%\Microsoft\Windows\Fonts\` 下，把你需要的字体拷贝一份到系统文件夹下就可以了。

记住以后不要再右键安装字体文件了。


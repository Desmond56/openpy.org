---
layout: ubuntu18.04
title: Gnome安装搜狗后无法输入中文
date: 2019-12-17 15:21:03
categories: 
- Linux
tags:
 - ubuntu
 - gnome
 - linux输入法
---

我用的是Gnome On Wayland 如果是这个桌面的话 是无法读取 ~/.xprofile 中的环境变量，所以请在/etc/environment中加入：
```bash
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
```
或者在登录时选择运行于Xorg的Gnome
参考文档地址: [archlinux-fcitx](https://wiki.archlinux.org/index.php/Fcitx_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))


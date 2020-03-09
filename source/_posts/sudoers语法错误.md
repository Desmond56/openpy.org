---
uuid: 56dfe95b-84c4-3c18-21ce-c7ea5a60a760
title: sudoers语法错误
categories:
  - Linux
  - Ubuntu
tags:
  - sudoers
  - sudo不能执行
comments: true
abbrlink: 275f2d5d
date: 2020-03-09 14:53:14
img:
---

# 误修改sudoers文件导致执行sudo时提示语法错误解决方案
#### 1. 直接切换到root用户修改(已知root密码)
```bash
# 切换到root用户
su
# 修改/etc/sudoers
vi /etc/sudoers
...
```
#### 2. 如果不知道root密码, 可重启进入恢复模式修改, 具体流程参照网络
#### 3. 通过pkexec命令修改
```bash 
# 如果为个人桌面Ubuntu系统, 可打开终端输入
pkexec visudo
# 这时候桌面会弹出输入密码的窗口要求输入密码
# 输入密码后就会进入一个nano的编辑窗口, 修改错误,保存, 问题解决
```

```bash
# 如果是Ubuntu server, 没有桌面环境, 应执行以下流程
# 打开两个终端, 分别连接该服务器
# 在终端1执行 
echo $$
# 会得到一个pid,接下来在终端2中执行
pkttyagent --process 1313
# 此时终端2会监听终端1 pkexec命令
# 在终端1执行以下命令
pkexec visudo
# 在终端2中输入用户密码
# 此时终端1就进入了nano编辑模式, 修改错误, 保存. 问题解决
```
终端1
```bash
echo $$
1313

pkexec visudo
```

终端2
```bash
pkttyagent --process 1313
==== AUTHENTICATING FOR org.freedesktop.policykit.exec ===
要以超级用户的身份运行“/usr/sbin/visudo” 程序，您必须通过验证
Authenticating as: avicii,,, (alex)
Password: 
```

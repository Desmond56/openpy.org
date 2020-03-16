---
uuid: 6d59dc20-cda2-6005-7b5d-1e2c858ffc5f
title: Ubuntu修改开机动画及紫色锁屏
categories:
  - Linux
  - Ubuntu
tags:
  - ubuntu开机动画
  - 紫色锁屏
img: 'https://cdn.openpy.org/ubuntu-logo.jpg'
abbrlink: ee4c5fa7
date: 2019-12-20 10:58:27
---
#### 1. Ubuntu修改开机动画
##### 1.1 安装全套动画
```shell script
sudo apt-get install plymouth-theme*
sudo update-alternatives --config default.plymouth # 输入编号选择一个。
sudo update-initramfs -u
sudo reboot
```
<!--more-->
此时开机动画已替换完成, ps:博主选的kubuntu or ubuntu-gnome-logo
#### 2. 替换紫色锁屏
在设置中设置的锁屏壁纸貌似只在锁屏的状态下会显示, 当解锁输入密码时,还是那个紫色锁屏,更改方法如下
```shell script
cd /usr/share/gnome-shell/theme
sudo cp ubuntu.css ubuntu.css.bak #备份防止改错
sudo vim ubuntu.css
```
找到lockDialogGroup所在的位置，直接改就好了。
```css
#lockDialogGroup {
   background: #2c001e url(file:///boot/lock.jpg); /* 路径为你选择的图片的路径*/
   background-repeat: no-repeat; /*由repeat修改为no-repeat*/
   /*下面这两行是新增的*/
   background-size: cover;
   background-position: center;
}
```
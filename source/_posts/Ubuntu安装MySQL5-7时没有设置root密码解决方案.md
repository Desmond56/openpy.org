---
uuid: c2e11b78-f4bc-54d1-e3c7-dbb10b30ab24
title: Ubuntu安装MySQL5.7时没有设置root密码解决方案
categories:
  - Linux
  - mysql
tags:
  - mysql5.7
abbrlink: 1121b615
date: 2019-12-17 15:33:19
---
```shell
$ sudo vim /etc/mysql/debian.cnf
# 找到password一行,复制密码
$ mysql -u debian-sys-maint -p 此长密码
mysql> use mysql;
mysql> update user set authentication_string=PASSWORD("你的密码") where user='root';
mysql> update user set plugin="mysql_native_password"; # 不用修改, 直接执行
mysql> flush privileges;
mysql> quit;
$ sudo service mysql restart
# 这个时候就可以用刚刚设置的root密码登录mysql了
```
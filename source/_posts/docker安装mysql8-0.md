---
title: docker安装mysql8.0
categories:
  - mysql
tags:
  - mysql
  - docker
  - mysql8.0
comments: true
abbrlink: 1493d95
date: 2020-05-22 18:06:38
img:
---
1. 先查看[docker-mysql版本](https://hub.docker.com/_/mysql?tab=description)
<!--more-->
2. 拉docker镜像到本地
```bash
sudo docker pull mysql:8.0.20
```
3. 宿主机创建配置文件夹
```bash
sudo mkdir -p /opt/docker-mysql/conf.d
```
4. 在创建的文件夹下添加 my.cnf 配置文件, 文件名结尾必须为.cnf, 以下为示例内容
```nginx
[mysqld]
# 表名不区分大小写
lower_case_table_names=1 
#server-id=1
datadir=/var/lib/mysql
#socket=/var/lib/mysql/mysqlx.sock
#symbolic-links=0
# sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```

5. 创建数据存储文件夹
```bash
mkdir -p /opt/docker-mysql/data
```

6. 运行mysql容器(xxxxxxxx为设置的root密码)
```bash
docker run --name mysql \
    --restart=always \
    -p 3306:3306 \
    -v /opt/docker-mysql/conf.d:/etc/mysql/conf.d \
    -v /opt/docker-mysql/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=xxxxxxxx \
    -d mysql:8.0.20
```

7. 常用命令
```bash
# 进入mysql容器
docker exec -it mysql bash
# 备份数据
docker exec mysql sh -c 'exec mysqldump --all-databases -uroot -p"123456-abc"' > /some/path/on/your/host/all-databases.sql
# 恢复数据
docker exec -i mysql sh -c 'exec mysql -uroot -p"123456-abc"' < /some/path/on/your/host/all-databases.sql
# 创建访问用户
create user 'node'@'172.17.0.1' identified by 'Asdfzxcv1234';
# 设置新用户访问权限
GRANT ALL ON *.* TO 'root'@'172.17.0.1';
```

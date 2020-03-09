---
uuid: 53c5ad62-0e2e-3b94-63e5-16c52138be2a
title: '添加当前用户到docker组,docker命令免sudo'
tags:
  - docker
categories:
  - docker
abbrlink: 8596c31f
date: 2019-12-17 15:28:22
---
#### 添加当前用户到docker组
如果想在当前用户输入docker命令时不输入docker,需要将当前用户添加到docker组当中
#### 1.查看docker组中用户列表
```shell
 sudo cat /etc/group | grep docker
```
最后一个 : 后面表示为docker组中的用户列表, 如果没有当前用户名, 则需要将当前用户加入docker组当中
#### 2. 添加当前用户到docker组
```shell
sudo gpasswd -a ${USER} docker
```
#### 3. 重启docker服务
```bash
sudo service docker restart
```
#### 4. 如果提示socket文件权限不足, 则给 .sock 文件增加如下权限
```bash
sudo chmod a+rw /var/run/docker.sock
```
#### 5. 重启docker服务
```bash
sudo service docker restart
```
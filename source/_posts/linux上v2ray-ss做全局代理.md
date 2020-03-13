---
uuid: a6092d3a-7a82-4a17-fc7e-3f893d2e25e2
title: linux上v2ray/ss做全局代理
categories:
  - Linux
tags:
  - v2ray
  - shadowsocks
  - 全局代理
comments: true
abbrlink: d5182360
date: 2020-03-13 14:32:03
img:
---
本文转发自[wonderkun's Blog](http://wonderkun.cc/2016/05/16/%E5%9C%A8linux%E4%B8%8A%E6%8A%8Ashadowsocks%E5%81%9A%E6%88%90%E5%85%A8%E5%B1%80%E4%BB%A3%E7%90%86/)
1. 默认系统已安装v2ray, shadowsocks等代理软件, 并且通过socks5监听了本机1080端口
2. 安装redsocks
```bash
sudo apt install redsocks

### redsocks常用命令
sudo service redsocks start     #打开redsocks
sudo service redsocks stop      #停止redsocks
sudo service redsocks restart   #重启redsocks

sudo systemctl enable redsocks  #开机启动redsocks
sudo systemctl disable redsocks  #停止开机启动redsocks
```
3. 配置redsocks
编辑redsocks配置文件*/etc/redsocks.conf* 找到redsocks
```bash
redsocks {
    /* `local_ip' defaults to 127.0.0.1 for security reasons,
     * use 0.0.0.0 if you want to listen on every interface.
     * `local_*' are used as port to redirect to.
     */
    local_ip = 127.0.0.1;
    local_port = 12345; //这个端口默认就行,只要跟你以后iptables,重定向的端口一样就ok
    
    // `ip' and `port' are IP and tcp-port of proxy-server
    // You can also use hostname instead of IP, only one (random)
    // address of multihomed host will be used.
    ip = 127.0.0.1; //本地shadowsocks客户端,地址就是127.0.0.1
    port = 1080;    //shadowsocks  客户端的端口,默认就是1080
    
    
    // known types: socks4, socks5, http-connect, http-relay
    type = socks5; //使用的代理类型
    
    // login = "foobar";
    // password = "baz";
}
```
 修改好配置文件后, 重启redsocks

4. 配置iptables, 将以下脚本保存为iptables.sh
```bash
#!/bin/bash
if [ $# -lt 1 ]

#不重定向目的地址为服务器的包
then
    echo -en "\n"

    echo "Iptables redirect script to support global proxy on ss for linux ... "
    echo -en "\n"
    echo "Usage : ${0} action [options]"
    echo "Example:"
    echo -en "\n"
    echo "${0} start server_ip To start global proxy"
    echo "${0} stop To stop global proxy"
    echo -en "\n"

else
    if [ ${1} == 'stop' ]
    then
        echo "stoping the Iptables redirect script ..."
        sudo iptables -t nat -F
   fi
   if     [ ${1} == 'start' ]
   then
       if    [ $# -lt 2 ]
       then
            echo -e "\033[49;31mPlease input the server_ip ...\033[0m"
       else
           ##不重定向目的地址为服务器的包  
           sudo iptables -t nat -A OUTPUT -d ${2} -j RETURN #请用你的shadowsocks服务器的地址替换$SERVER_IP
           # #不重定向私有地址的流量
           sudo iptables -t nat -A OUTPUT -d 10.0.0.0/8 -j RETURN
           sudo iptables -t nat -A OUTPUT -d 172.16.0.0/12 -j RETURN
           sudo iptables -t nat -A OUTPUT -d 192.168.0.0/16 -j RETURN

           #不重定向保留地址的流量,这一步很重要
           sudo iptables -t nat -A OUTPUT -d 127.0.0.0/8 -j RETURN

            # #重定向所有不满足以上条件的流量到redsocks监听的12345端口
           sudo iptables -t nat -A OUTPUT -p tcp -j REDIRECT --to-ports 12345 #12345是你的redsocks运行的端口,请根据你的情况替换它
     fi
  fi
fi
```
5. 将上面代码保存好后, 运行脚本启动就可以设置全局代理了, 启动和停止脚本如下:
```bash
./iptables.sh   start  ip     #ip是你的shadowsocks服务器的ip,开启全局代理 
./iiptables.sh  stop   #结束全局代理,这句是不用全局代理之后,必须运行的,否则是没有办法上网的 
```

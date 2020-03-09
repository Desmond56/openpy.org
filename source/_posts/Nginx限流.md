---
uuid: 23cbefd3-15b2-57f2-f42a-a74841c5b8c8
title: Nginx限流
categories:
  - Linux
  - nginx
tags:
  - nginx限流
  - nginx
abbrlink: c8db2ce0
date: 2019-12-16 19:22:58
---
### Nginx限流模块包含
- 连接数限流模块 **ngx_http_limit_conn_module**
- 漏桶算法实现的请求限流模块 **ngx_http_limit_req_module**
### **ngx_http_limit_conn_module**
我们经常会遇到这种情况，服务器流量异常，负载过大等等。对于大流量恶意的攻击访问，会带来带宽的浪费，服务器压力，影响业务，往往考虑对同一个ip的连接数，并发数进行限制。
ngx_http_limit_conn_module 模块来实现该需求。该模块可以根据定义的键来限制每个键值的连接数，如同一个IP来源的连接数。并不是所有的连接都会被该模块计数，只有那些正在被处理的请求（这些请求的头信息已被完全读入）所在的连接才会被计数。
我们可以在nginx_conf的http{}中加上如下配置实现限制：
```nginx
# 限制每个用户的并发连接数，取名one
limit_conn_zone $binary_remote_addr zone=one:10m;

# 配置记录被限流后的日志级别，默认error级别
limit_conn_log_level error;
# 配置被限流后返回的状态码，默认返回503
limit_conn_status 503;
```
然后在server{}里加上如下代码：
```nginx
#限制用户并发连接数为1
limit_conn one 1;
```
另外刚才是配置针对单个IP的并发限制，还是可以针对域名进行并发限制，配置和客户端IP类似。
```nginx
#http{}段配置
limit_conn_zone $ server_name zone=perserver:10m;
#server{}段配置
limit_conn perserver 1;
```
### **ngx_http_limit_req_module**
上面我们使用到了ngx_http_limit_conn_module 模块，来限制连接数。那么请求数的限制该怎么做呢？这就需要通过ngx_http_limit_req_module 模块来实现，该模块可以通过定义的键值来限制请求处理的频率。
特别的，可以限制来自单个IP地址的请求处理频率。限制的方法是使用了漏斗算法，每秒固定处理请求数，推迟过多请求。如果请求的频率超过了限制域配置的值，请求处理会被延迟或被丢弃，所以所有的请求都是以定义的频率被处理的。
在http{}中配置
```nginx
#区域名称为one，大小为10m，平均处理的请求频率不能超过每秒一次。
limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
```
在server{}中配置
```nginx
#设置每个IP桶的数量为5
limit_req zone=one burst=5;
```
上面设置定义了每个IP的请求处理只能限制在每秒1个。并且服务端可以为每个IP缓存5个请求，如果操作了5个请求，请求就会被丢弃。
### 参考文档
[https://colobu.com/2015/10/26/nginx-limit-modules/](https://colobu.com/2015/10/26/nginx-limit-modules/)
[朱小厮的博客]([https://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247488251&idx=1&sn=0ea27c9b8d3a0b1656137162ccf39d80&chksm=fb0bf86fcc7c71790c67a58efb1f8114abaeb099bf1e453a33b5d1905c8ee6e7b27eb57fd61a&mpshare=1&scene=1&srcid=&sharer_sharetime=1576477648655&sharer_shareid=714f27d4f289ce96efe7800acadcd2d8#rd](https://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247488251&idx=1&sn=0ea27c9b8d3a0b1656137162ccf39d80&chksm=fb0bf86fcc7c71790c67a58efb1f8114abaeb099bf1e453a33b5d1905c8ee6e7b27eb57fd61a&mpshare=1&scene=1&srcid=&sharer_sharetime=1576477648655&sharer_shareid=714f27d4f289ce96efe7800acadcd2d8#rd)
)
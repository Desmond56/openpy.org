---
uuid: 2ec405e2-74ba-2291-aa50-7328ccc89e35
title: go-micro增加插件
tags:
  - go-micro
categories:
  - 后端
  - Golang
abbrlink: 8f4538c0
date: 2019-12-23 18:59:44
---
增加etcdv3
在$GOPATH/src/github.com/micro/micro下新建plugins.go
```golang
package main

import (
    _ "github.com/micro/go-plugins/registry/etcdv3"
)
```
```shell script
  $ go build -i -o micro ./main.go ./plugins.go
  $ mv micro $GOBIN/
  $ micro --registry=etcdv3 list services
  lp.srv.eg1
  lp.srv.eg2 
  #　报错trace包重复, 将报错路径下的trace包删除
```
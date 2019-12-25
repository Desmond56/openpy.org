---
title: go-micro增加插件
date: 2019-12-23 18:59:44
tags:
- go-micro
categories:
- 后端
- Golang
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
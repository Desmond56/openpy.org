---
title: python防止对象无对应方法或变量报错
categories:
  - 后端
  - python
tags:
  - hasattr
comments: true
abbrlink: d7d50464
date: 2020-01-08 16:14:24
img:
---
1. 为了防止打印对象中不包含的属性时报错, 应先判断对象中是否有此属性, 有才执行相关代码
```python
# obj为对象名称, name为方法名称
if hasattr(obj, name):
    print("有这个属性")
else:
    print("没有这个属性")
```

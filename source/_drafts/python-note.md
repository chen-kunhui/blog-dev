---
title: python 笔记
draft: true
date: 2018-08-23 22:49:01
tags:
  - python
  - help
categories:
  - python
---

# 动态获取对象的方法和属性等
> https://www.cnblogs.com/zh1164/p/6031464.html

动态地获取方法和属性可以帮助我们快速地查阅对象有哪些可以使用的方法和属性，从而快速地了解对象。

1. 返回包含obj大多数属性名的列表（会有一些特殊的属性不包含在内）。obj的默认值是当前的模块对象

```
# dir(obj)
>>> list = [1,2,3]
>>> dir(list)
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__iter__', '__le__', '__len__', '__lt__' ... ]
```

2. 获取对象的类型

```
# type(obj)
>>> list = [1,2,3]
>>> list.__class__
<type 'list'>
>>> type(list)
<type 'list'>
```

3. 查看文档

```
# help(obj.method)
>>> list = [1,2,3]
>>> help(list.)
```
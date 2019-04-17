---
title: puthon 笔记
tags:
  - ruby
  - help
draft: true
date: 2018-08-23 22:49:01
categories:
  -ruby
---




require 'test/unit'
表示测试的类必须是Test::Unit::TestCase的子类，含有断言的方法名必须以test开头。主要是因为Test::Unit使用反射来查找需要运行的测试。只有test开头才符合条件

# 动态获取对象的方法和属性等

- `obj.methods` : 获取对象的方法和属性
- `obj.public_methods` : 获取对象的非私有方法 
- `obj.class` : 获取对象的类型
- `obj.instance_of? class_name` ： 判断某一对象是否属于某一类型

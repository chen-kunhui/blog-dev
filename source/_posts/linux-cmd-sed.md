---
title: linux 命令之 sed
date: 2019-04-18 22:00:04
tags:
categories:
  - linux
  - cmd
---

# sed 命令

`sed`，`awk` 和 `grep` 通常被称为文本处理三剑客。`sed` 是 Stream Editor（字符流编辑器） 的缩写。

通常 `sed` 命令并不会改变原文件，要改变原文件可通过指定 `-i` 选项，比如 `sed -i ...`

语法如下：

    $ sed [选项] [sed 内置命令符] [要处理的文件]
    $ [要处理标准输出] | sed [选项] [sed 内置命令符]

选项：

    -e    指定脚本来处理文本
    -f    指定脚本文件来处理文本
    -n    仅显示处理过的文本
    -h    显示帮助
    -V    显示版本

sed 内置命令符:

    a     新增
    c     替换
    d     删除
    i     插入
    p     打印
    s     替换

> NOTE: 下面的例子中 example-file 为要处理的文件

## 搜索并处理

1.仅对匹配到的行进行处理

    sed '/正则表达式/{处理语句}'

    如： 将包含有 hello 的行中的 abc 替换为 bcd
    $sed -e '/hello/{s/abc/bcd/g}'

    将包含有 hello 的行中的 abc 删除
    $sed -e '/hello/{s/abc//g}'

## 多次编辑

可通过重复指定 -e 选项，对文本进行多次操作

    如：将 ab 开头的行中的name 替换为 name：haha 并删除以cd开头的行

    $sed -e '/^ab/{s/name/name: haha/g}' -e '/^cd/d' example-file

## 部分替换（s）

    sed 's/要被取代的字串/新的字串/g'  ---> 全局替换
    sed 's/要被取代的字串/新的字串/'   ---> 只替换每行第一个匹配到的结果

    如：将文件中的 hello 替换为 hi

    $sed -e 's/hello/hi/' example-file
    不加 g 将仅替换每行中的第一个 hello，要将一行中的多个hello替换需在末尾加 g

## 插入行（a/i）

1.在第二行的下方插入内容为 new line 的行

    $sed -e '2a new line' example-file

2.在第二行上方插入内容为 new line 的行

    $sed -e '2i new line' example-file

3.在包含 ab 行的上方插入内容为 test test 的新行( sed -e '/正则表达式/i 插入的字符')

    $sed -e '/ab/i test test' example-file

## 删除行（d）

1.删除第二行

    $sed -e 2d example-file

2.将第一到第三行删除

    $sed -e '1,3d' example-file

3.将第三行到最后一行删除

    $sed -e '3,$d' example-file

4.删除匹配 ab 的行( sed -e '/正则表达式/d')

    $sed -e '/ab/d' example-file

## 整行或多行替换（c）

1.将第二行替换为 test test

    $sed -e '2c test test' example-file

2.将第二行到第三行替换为 test test

    $sed -e '2,3c test test' example-file

3.将匹配上 ab 的行替换为 test test( sed -e '/正则表达式/c  替换后的字符')

    $sed -e '/ab/c test test' example-file

## 仅显示部分行（p）

1.仅显示第三到第五行

    $sed -n '3,5p' example-file

2.仅显示匹配 hello 的行

    $sed -n '/hello/p' example-file

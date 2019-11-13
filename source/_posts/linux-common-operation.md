---
title: linux-common-operation
mathjax: false
date: 2019-11-13 20:38:34
tags:
  - cmd
  - linux
categories:
  - linux
---

# 添加新用户

useradd 和 adduser 的区别
| comamd | description |
| --- | --- |
| useradd | useradd是一个linux命令，仅添加用户，不会在 /home 下创建目录 |
| adduser | adduser是一个perl 脚本，它会添加用户和用户组，并在 /home 目录创建目录 |

example:
```shell
>_$ adduser xxx
  or
>_$ useradd xxx
```

# 给普通用户添加 sudo 权限

```
echo 'chenkh    ALL=(ALL:ALL) ALL' >> /etc/sudoers
```

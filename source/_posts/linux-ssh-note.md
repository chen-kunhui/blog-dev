---
title: ssh 笔记
mathjax: false
date: 2019-04-29 21:53:54
tags:
categories:
  - linux
  - cmd
---

# 小记
生成 ssh key

    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

将 ssh key 添加到目标主机，从而实现免密登陆

    ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.0.22

> 该操作相对于将 ssh 公钥复制粘贴到 192.168.0.22 机器 root 用户 ~/.ssh/authorized_keys 文件中

远程执行 shell 命令

    ssh  root@192.168.0.22 "echo hello"

登陆远程主机

    ssh root@192.168.0.22

关闭ssh连接时提示的yes和no

    ssh -o StrictHostKeyChecking=no 58.221.186.137

或者

    cat > ~/.ssh/config << end
    UserKnownHostsFile /dev/null
    ConnectTimeout 15
    StrictHostKeyChecking no
    end


# sshd 配置文件 sshd_config

| 配置 | 值 | 说明 |
| --- | --- | --- |
| ClientAliveInterval | int | 指定服务器端向客户端请求消息的时间间隔 |
| ClientAliveCountMax | int | 服务器发出多少次请求后客户端没有响应则自动断开|
| PermitRootLogin | yes/no | 是否允许root用户远程登录 |

# 查看登陆历史

| 命令 | 作用 |
| --- | --- |
| last | 查看近期的历史 |

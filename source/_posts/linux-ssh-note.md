---
title: ssh 笔记
mathjax: false
date: 2019-04-29 21:53:54
tags:
categories:
  - linux
  - cmd
---

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




---
title: scp, sftp 传输文件
mathjax: false
date: 2019-04-29 22:36:33
tags:
categories:
  - linux
  - cmd
---

# scp

下载文件到本地

    # 拷贝 10.183.234.111 主机的 /home/username/test.log 文件到本地的当前目录下
    scp username@10.183.234.111/home/username/test.log ./

    # 拷贝 10.183.234.111主机的 /home/username/dir 目录及内容到本地的当前目录下
    scp -r username@10.183.234.111/home/username/dir ./

上传文件到远程机器

    # 将本地当前目录下的 test.rb 文件上传到 10.183.234.111 主机的 /home/username 目录下
    scp test.rb username@10.183.234.111/home/username

    # 将本地当前目录下的 test_dir 目录及内容上传到 10.183.234.111 主机的 /home/username 目录下
    scp -r test_dir username@10.183.234.111/home/username

# sftp

> 引用 https://www.cnblogs.com/Xbingbing/p/8504818.html

sftp 是一个交互式文件传输程式。它类似于 ftp, 但它进行加密传输，比FTP有更高的安全性。

## 常用登陆方式：

格式：sftp user@host

通过 sftp 连接 host，端口为默认的 22，指定用户 user。

## 查看sftp支持的命令

使用help命令，查看支持的命令，如：

    help

(其中命令前面有“l”表示本地执行，其他表示在所登录的远程主机上面执行)

## 基本的使用

sftp主要是用来传输文件的，包括上传文件(从本机到远程主机) ，下载文件(从远程主机到本机)。

### 文件下载

    get [-Ppr] remote [local]

    # 将远程当前目录下的文件test.cpp下载到本地当前目录的Project文件夹中。
    get test.cpp Project/

    # 将远程目录 abc 下载到本地当前目录的Project文件夹中。
    get -r abc Project/ 或者 get -R abc Project/



### 文件上传

    put [-Ppr] local [remote]

    #将本地/home/Xbingbing/Software/目录下的ios文件传送到远程登陆主机的/home/Xbingbing/Blog/目录下。
    put /home/Xbingbing/Software/RHEL_5.5x86_64.iso /home/Xbing bing/Blog/

    # 将本地目录 abc下的内容推送到远程的 Project 目录下
    put -r abc Project/ 或者 put -R abc Project/

### 命令列表

退出命令：

    bye                                Quit sftp
    exit                               Quit sftp
    quit                               Quit sftp

其他命令：

    progress                           Toggle display of progress meter
    version                            Show SFTP version
    !command                           Execute 'command' in local shell
    !                                  Escape to local shell
    ?                                  Synonym for help
    help                               Display this help text

上传下载：

    put [-afPpRr] local [remote]       Upload file
    get [-afPpRr] remote [local]       Download file
    reget [-fPpRr] remote [local]      Resume download file
    reput [-fPpRr] [local] remote      Resume upload file

对远程主机操作：

    cd path                            Change remote directory to 'path'
    chgrp grp path                     Change group of file 'path' to 'grp'
    chmod mode path                    Change permissions of file 'path' to 'mode'
    chown own path                     Change owner of file 'path' to 'own'
    rename oldpath newpath             Rename remote file
    rm path                            Delete remote file
    rmdir path                         Remove remote directory
    symlink oldpath newpath            Symlink remote file
    mkdir path                         Create remote directory
    pwd                                Display remote working directory
    df [-hi] [path]                    Display statistics for current directory or
                                       filesystem containing 'path'

对本地操作：

    lcd path                           Change local directory to 'path'
    lls [ls-options [path]]            Display local directory listing
    lmkdir path                        Create local directory
    ln [-s] oldpath newpath            Link remote file (-s for symlink)
    lpwd                               Print local working directory
    ls [-1afhlnrSt] [path]             Display remote directory listing
    lumask umask                       Set local umask to 'umask'

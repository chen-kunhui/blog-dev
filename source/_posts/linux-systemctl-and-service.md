---
title: linux systemctl 管理服务 与 service 管理服务
mathjax: false
date: 2019-05-18 09:28:05
tags:
categories:
  - linux
---

较新的发行版一般使用 systemctl

# 查看系统以生什么命令来管理服务

```bash
ps -p 1
```

# service 命令管理服务

以 elasticsearch 为例

```
sudo service elasticsearch start
sudo service elasticsearch stop
sudo service elasticsearch restart
sudo service elasticsearch status
```

设置开机启动
```
sudo update-rc.d elasticsearch defaults 95 10
```
启动脚本位置
```
/etc/init.d/elasticsearch
```
> `sudo service elasticsearch start` 其实相当于 `bash /etc/init.d/elasticsearch start`

# systemctl 命令管理服务

```
sudo systemctl start elasticsearch.service
sudo systemctl stop elasticsearch.service
sudo systemctl restart elasticsearch.service
sudo systemctl status elasticsearch.service
```

设置开机启动
```
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch.service
```

启动文件位置
```
/usr/lib/systemd/system/elasticsearch.service
```

查看后台日志
```
sudo journalctl -f
sudo journalctl --unit elasticsearch
sudo journalctl --unit elasticsearch --since  "2016-10-30 18:17:16"
```

> https://www.freedesktop.org/software/systemd/man/journalctl.htmlß

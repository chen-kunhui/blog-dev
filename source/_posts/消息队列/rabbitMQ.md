---
title: RabbitMQ
date: 2023-04-17 22:00:00
updated: 2023-04-17 22:00:00
tags:
  - 消息队列
  - db
categories:
  - db
---

# 安装

```
apt-get install rabbitmq-server
```

## 启动、停止、查看运行状态

```
service rabbitmq-server status
service rabbitmq-server stop
service rabbitmq-server start
```

## 查看已安装插件

```
rabbitmq-plugins list
```

## 其他命令

rabbitmq-plugins enable rabbitmq_management  启用一个插件

> rabbitmq_management 插件启用后可以通过 15672 端口访问web管理页面，使用guest,guest 进行登陆web页面

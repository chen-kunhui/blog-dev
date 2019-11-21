---
title: linux-etcd-install
mathjax: false
date: 2019-11-15 20:17:45
tags:
  - linux
  - docker
  - k8s
categories:
  - k8s
---

[etcd](https://etcd.io/) 是一个键值存储仓库，用于配置共享和服务发现

# etcd 的安装

## 1. [下载源码安装包](https://github.com/etcd-io/etcd/releases)解压安装

```shell
$ tar -xzvf etcd-v3.2.28-linux-amd64.tar.gz -C /usr/local/src/
$ tree -L 1 etcd-v3.2.28-linux-amd64
$ ln -s /usr/local/src/etcd-v3.2.28-linux-amd64/etcd /usr/local/bin/
$ ln -s /usr/local/src/etcd-v3.2.28-linux-amd64/etcdctl /usr/local/bin/
$ etcd --version
$ etcdctl --version
```

## 2. 启动 etcd 服务

```shell
$ etcd --data-dir /home/chenkh/default.etcd
```

## 3. 存一个值与取一个值

```shell
$ ETCDCTL_API=3 etcdctl --endpoints=127.0.0.1:2379 put foo "hello world"
$ ETCDCTL_API=3 etcdctl --endpoints=127.0.0.1:2379 get foo
```
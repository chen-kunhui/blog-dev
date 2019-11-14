---
title: docker-kubectl-install
mathjax: false
date: 2019-11-14 21:30:20
tags:
  - docker
  - k8s
  - linux
categories:
  - docker
---

# kubectl 安装

> 原文：https://www.kubernetes.org.cn/installkubectl

kubectl 是 Kubernetes 命令行工具,可以在Kubernetes上部署和管理应用程序，可以检查集群资源，创建，删除和更新组件

以下是安装kubectl的几种方法

## 第二种：直接下载 kubectl 二进制文件

### 1.下载可执行文件

这里以 v1.16.3 版本为例，最新的稳定版本号可访问[这里](https://storage.googleapis.com/kubernetes-release/release/stable.txt)获取

#### MacOS 版本

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.16.3/bin/darwin/amd64/kubectl
```

#### Linux 版本

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.16.3/bin/darwin/amd64/kubectl
```

#### window 版本

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.16.3/bin/windows/amd64/kubectl.exe
```

### 2. 将可执行文件添加可执行权限，然后添加到 PATH 中，或放置到 /usr/local/bin 这样的位置

```
chmod a+x kubectl
mv kubectl /usr/local/bin
kubectl version
```

## 第一种：通过包管理工具安装

### ubuntu

如果您在Ubuntu或其他支持快照包管理器的Linux发行版之一，您可以使用以下安装
```
sudo snap install kubectl --classic
```

### MacOS

#### 通过 Docker Desktop 安装

打开 Docker Desktop > 选择 preferences > Kubernetes > 勾选 Enable Kubernetes

#### 第二种命令行方式
```
brew install kubectl
```

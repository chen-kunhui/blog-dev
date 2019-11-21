---
title: k8s-minikube-use
mathjax: false
date: 2019-11-15 22:13:52
tags:
categories:
---

# [安装](https://minikube.sigs.k8s.io/docs/start/linux/)

安装前需先[安装 kubectl](./k8s-kubectl-install.md)

安装 minikube

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_1.5.2.deb \
 && sudo dpkg -i minikube_1.5.2.deb
```

如需更新minikube，需要更新 minikube 安装包

minikube delete 删除现有虚机，删除 ~/.minikube 目录缓存的文件
重新创建 minikube 环境

# 相关资料

> - https://kubernetes.io/docs/tasks/tools/install-minikube/
> - https://yq.aliyun.com/articles/221687
> - https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux

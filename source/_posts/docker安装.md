---
title: docker 安装
mathjax: false
date: 2019-11-13 21:02:19
updated: 2019-11-13 21:02:19
tags:
  - docker
categories:
  - docker
---

# docker [安装](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-from-a-package)

## 1. [下载](https://download.docker.com/linux/ubuntu/dists/)安装包

> 请根据系统版本下载对于的 deb 包，可以通过 `sudo lsb_release -a` 查看系统版本

```shell
$ wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/docker-ce_18.03.1~ce~3-0~ubuntu_amd64.deb
```

## 2. 安装

```shell
$ sudo dpkg -i docker-ce_18.03.1~ce~3-0~ubuntu_amd64.deb 
```

## 3. 运行 hello-world 示例

```shell
$ sudo docker run hello-world
```

## 4. 将普通用户添加到 docker 组，避免每次都需要使用 sodu 命令

```
sudo usermod -aG docker your_username
newgrp docker
```

## 5. 修改 docker hub 仓库地址

有时默认的 docker hub 地址下载镜像时会很慢，可以更改为国内的镜像

### 1. 在 `/etc/docker/daemon.json` 加入以下内容

```json
{

   "registry-mirrors": ["https://registry.docker-cn.com"]

}
```

### 2. 重启 docker

```
sudo service docker restart
```
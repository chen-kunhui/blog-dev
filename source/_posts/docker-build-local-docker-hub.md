---
title: docker-build-local-docker-hub
mathjax: false
date: 2019-11-13 23:05:14
tags:
  - docker
categories:
  - docker
---

# 搭建本地的 docker hub

## 1.拉取 registry 镜像

```shell
$ docker pull registry
```

## 2.启动容器

```shell
$ docker run -d -p 5000:5000 --restart=always --name registry registry
```

> --restart=always 可以理解为每次重启docker时都重启容器

默认情况下，仓库会被创建在容器的 /var/lib/registry 目录下。你可以通过 -v 参数来将镜像文件存放在本地的指定路径， 例如

```
$ docker run -d \
    -p 5000:5000 \
    -v /opt/data/registry:/var/lib/registry \
    registry
```

## 3.本地创建一个镜像

```
$ cat > Dockerfile <<EOF
FROM ubuntu
CMD echo "hello ....."
EOF
$ docker build ./ -t 127.0.0.1:5000/hello:1.0.1
```

## 4.将镜像推到本地的 docker hub

```
$ docker push 127.0.0.1:5000/hello:1.0.1
```

## 5.查看本地的 docker hub 上有那些仓库

```
$ curl 127.0.0.1:5000/v2/_catalog
{"repositories":["hello"]}
```

## 6.将本地制作的镜像删除，从搭建的 docker hub 上重新拉取

```
$ docker rmi 127.0.0.1:5000/hello:1.0.1
$ docker pull 127.0.0.1:5000/hello:1.0.1
```

## 7.测试新拉下来的镜像

```
$ docker run 127.0.0.1:5000/hello:1.0.1
hello .....
```

> [Registry API](https://docs.docker.com/registry/spec/api/)
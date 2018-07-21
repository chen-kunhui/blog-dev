---
title: elasticsearch 安装与启动
tags:
  - elasticsearch
  - Base
date: 2018-07-18 22:55:27
---


# 概述

Elasticsearch 是一个分布式、可扩展、实时的搜索与数据分析引擎，同时它也是 ELK 日志系统的一部分。

# 安装前准备

## 安装 JDK & 配置 JAVA_HOME 

1. 到[Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载 JDK
```bash
wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.tar.gz
```
- 参数说明：
  - –no-check-certificate #不检查证书
  - --no-cookies          #不使用 cookies.
  - --header String       #设置请求头，模拟一个cookie

2. 解压 JDK 源码包到`/usr/local/java`路径下
```
sudo tar -zxvf jdk-8u181-linux-x64.tar.gz -C /usr/local/java
```

3. 在 `/etc/profile` 文件末尾加上：
```
export JAVA_HOME=/usr/local/java/jdk1.8.0_181 
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH
```
4. 运行以下命令，是修改后的文件生效
```
source /etc/profile
```

5. 测试 JDK 是否安装成功,在任意位置输入：
```
# 如果显示java版本则代表安装成功了
java -version
```

# 安装 & 运行

1. [下载源码包](https://www.elastic.co/downloads/elasticsearch)
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.1.tar.gz
```

2. 解压
```
tar -zxvf elasticsearch-6.3.1.tar.gz
```

3. 启动 elasticsearch
```
cd elasticsearch-6.3.1/
bin/elasticsearch
```

4. 测试是否启动成功
```
curl http://localhost:9200/
```
- 得到以下输出，表示启动成功：

    {
    "name" : "zmv4Idf",
    "cluster_name" : "elasticsearch",
    "cluster_uuid" : "oGYeMyYbTsKKIhms0yh8Pw",
    "version" : {
      "number" : "6.3.1",
      "build_flavor" : "default",
      "build_type" : "tar",
      "build_hash" : "eb782d0",
      "build_date" : "2018-06-29T21:59:26.107521Z",
      "build_snapshot" : false,
      "lucene_version" : "7.3.1",
      "minimum_wire_compatibility_version" : "5.6.0",
      "minimum_index_compatibility_version" : "5.0.0"
    },
    "tagline" : "You Know, for Search"
    }

# 扩展阅读

- [elasticsearch 文档](https://www.elastic.co/guide/cn/index.html)
- [elasticsearch 参考](https://es.xiaoleilu.com/010_Intro/00_README.html)

---
title: influxdb
date: 2018-10-17 18:57:15
updated: 2018-10-17 18:57:15
tags:
  - influxdb
categories:
  - db
---

# influxdb

[InfluxDB]是一个开源的时序数据库，使用GO语言开发，特别适合用于处理和分析资源监控数据这种时序相关数据。

> influxdb 文档： https://docs.influxdata.com/influxdb/v1.6/introduction/

## 名词及相关概念

> 官方关键概念解释： https://docs.influxdata.com/influxdb/v1.6/concepts/key_concepts/
> 官方术语解释：https://docs.influxdata.com/influxdb/v1.6/introduction/

### database

表示数据库

### measurement

influxdb 中没有表的概念，将表称为 measurement ，所以influxdb中没有创建表的语法，通常通过直接写入数据对应的 measurement 也就生成了

### points

在 influxdb 中，将一行数据成为points

### series

所有在数据库中的数据，都需要通过图表来展示，而这个series表示这个表里面的数据，可以在图表上画成几条线：通过tags排列组合算出来。具体可以通过SHOW SERIES FROM "表名" 进行查询。

### 数据构成

points 由时间（time），数据（fields），标签（tags）组成
- time ： 每条数据记录时间，是数据库中的主索引（会自动生成）
- fields ： 各种记录的值（没有索引属性），比如 温度，湿度
- tags： 各种有索引的属性，比如 地区，海拔

### 保存策略
用来规定数据在数据库中保存多久，每个库中都会有一个默认的保存策略，当插入数据没有制定保存策略时，就使用默认的保存策略，通常使用方式为`保存策略.表名`，**指定了什么保存策略存储数据，取的时候也要通过指定对应的保存策略取数据**，否则取不到数据

### 连续查询
通常与保存策略一起使用以降低InfluxDB的系统占用量。它是在数据库中按你指定的时间间隔，每隔一段时间执行一次的一条sql语句，通常做法是从某张 measurement 中取想要的数据，插入到另外一张 measurement 中。

## 安装启动与卸载

> 官方指导文档： https://docs.influxdata.com/influxdb/v1.3/introduction/installation/

> *以下记录 Ubuntu & Debian 的源码安装过程，其他系统可参考官方文档*

### 通过下载`.tar.gz` 的方式

#### 安装

只需要下载并解压[源码包](https://portal.influxdata.com/downloads#influxdb)即可
```
wget https://dl.influxdata.com/influxdb/releases/influxdb-1.6.3_linux_amd64.tar.gz
tar -xzvf influxdb-1.6.3_linux_amd64.tar.gz && cd influxdb-1.6.3-1
```

#### 启动

启动脚本在解压目录的`usr/bin`文件夹下`influxd`为服务端，`influx`为客户端，需要将服务段启动起来后才能才作数据库。

**tips:** 
- `influx_inspect` 为 influxdb 的查看工具
- `influx_stress` 为 influxdb 的压力测试工具
- `influx_tsm` 为 influxdb 的数据库转换工具
- 解压目录下的 `etc/influxdb/influxdb.conf`文件并没有被使用，可通过在解压目录下运行`./usr/bin/influxd config` 查看默认的配置信息

```sh
# 在解压目录下运行启动服务进程
./usr/bin/influxd
# 在解压目录下运行进入到influxdb
./usr/bin/influx
```

##### 数据存储位置
启动后，influxdb 会在用户的根目录（～/）下，生成 `.influxdb` 文件夹和 `.influx_history` 文件

- `.influxdb` 主要用于存放数据，包括`data` `meta` `mal` 三个文件夹
  - `data` : 存放最终存储的数据
  - `meta` : 存放数据库元数据
  - `mal` ： 存放预写日志文件
- `.influx_history` : 记录 influxdb 的操作记录

#### 卸载

删除解压的文件夹以及`~/.influxdb` 和 `~/.influx_history` 文件

### 通过 .deb 的方式

#### 安装

下载并使用 `sudo dpkg -i` 命令安装即可
```sh
# 下载 deb 包
wget https://dl.influxdata.com/influxdb/releases/influxdb_1.6.3_amd64.deb
# 安装
sudo dpkg -i influxdb_1.6.3_amd64.deb
```

#### 启动

安装好后，influxdb 会在 `/usr/bin`目录下安装好`influxd` `influx` `influx_inspect` `influx_stress` `influx_tsm`命令

和上面一样通过运行对应的命令做对应的事情即可，不同的是这样的安装是系统级的安装，还可以通过下面的命名管理influxdb
```sh
# 后台启动influxdb
sudo service influxdb start
# 重启
sudo service influxdb restart
# 查看 influxdb 运行状态
sudo service influxdb status
# 停止 influxdb 进程
sudo service influxdb stop
# 进入到influxdb
influx
```
**tips:** 
- 默认使用的配置文件为 `/etc/influxdb/influxdb.conf`
- 数据保存在 `/var/lib/influxdb` 下
- `/etc/default/influxdb` 可用于配置启动时命令行参数

#### 卸载

```
sudo dpkg --purge influxdb
sudo rm -rf /var/lib/influxdb/
```

## 使用

> 下文的操作皆在 influxdb 客户端中进行,请运行`influx`命令进入客户端

### 数据库的增加删除

```sh
# 创建数据库
CREATE DATABASE mydb
# 显示所有数据库
SHOW DATABASE
# 使用数据库
USE mydb
# 删除数据库
DROP DATABASE mydb
```

### 表、数据的增加删除

> 在做以下操作前请先使用 `USE` 命令使用某个数据库

influxdb 中没有特定创建表的语法，通过插入一条数据即可生成对应的表和数据，通过使用 Line Protocol 格式书写插入数据的格式。

> Line Protocal 资料: https://docs.influxdata.com/influxdb/v1.6/write_protocols/line_protocol_reference/#syntax

创建数据的语法通常为：
```
INSERT <measurement>[,<tag-key>=<tag-value>...] <field-key>=<field-value>[,<field2-key>=<field2-value>...] [unix-nano-timestamp]
```
第一值为表明接着逗号，逗号之后是**tag**由多个以逗号隔开`key=value`形式组成(*这里的key和value都必须为string*)，接着是一个空格，空格之后是**field**也是由多个`key=value`形式组成（*这里的key必须为string，value可以是string，floats，int或booleans*），接着又是一个空格，空格后为时间用于替换调自动生成的时间戳，不指定则自动生成

```sh
# 创建一张名为 cpu的表 , 包含 host 和 region 两个 tag，和一个value的field
INSERT cpu,host=serverA,region=us_west value=0.64
# 插入数据并指定数据记录时间
INSERT cpu,host=serverB,region=us_west value=0.99 1539788361141849675
# 显示所有 表
SHOW MEASUREMENTS
# 删除一张表
DROP MEASUREMENTS cpu
```

**数据查询语法与mysql一样**

> 查询语法指导：https://docs.influxdata.com/influxdb/v1.6/concepts/crosswalk/

### 用户管理

```sh
# 显示所有用户
SHOW USERS
# 创建用户
CREATE USER "username" WITH PASSWORD 'password'
# 创建管理员用户
CREATE USER "username" WITH PASSWORD 'password' WITH ALL PRIVILEGES
# 删除用户
drop user 'username'
```
influxdb 的用户权限分为`READ`,`WRITE`,`ALL PRIVILEGES`三种权限，可通过`CREATE USER "username" WITH PASSWORD 'password' WITH ALL PRIVILEGES`的方式在创建用户时，指定权限，

默认influxdb并没有开启认证，通过修改配置文件开启权限验证。将`auth-enabled = true`,然后通过`influx  -host ‘localhost‘ -port ‘8086‘ -username 'username' -password 'password'` 的方式登陆


### 保存策略的使用

```sh
# 查看现有的保存策略
SHOW RETENTION POLICIES ON mydb
```
```sh
# 创建保存策略
CREATE RETENTION POLICY "2_hours" ON "mydb" DURATION 2h REPLICATION 1
# 创建一个保存策略并设为默认
CREATE RETENTION POLICY "3_hours" ON "mydb" DURATION 3h REPLICATION 1 DEFAULT
```
```sh
# 修改保存策略为默认保存策略
ALTER RETENTION POLICY "2_hours" on "mydb" DEFAULT
# 修改保存策略的时间
ALTER RETENTION POLICY "2_hours" on "mydb" DURATION 4h
# 修改保存策略的时间并设为默认
ALTER RETENTION POLICY "2_hours" on "mydb" DURATION 2h DEFAULT
```
```sh
# 删除保存策略
DROP RETENTION POLICY "2_hours" ON "mydb"
```

```sh
# 查询时使用保存策略
SELECT * FROM "2_hours".cpu
```
### 连续查询的使用

```sh
SHOW CONTINUOUS QUERIES
```

```sh
CREATE CONTINUOUS QUERY <cq_name> ON <database_name>
[RESAMPLE [EVERY <interval>] [FOR <interval>]]
BEGIN SELECT <function>(<stuff>)[,<function>(<stuff>)] INTO <different_measurement>
FROM <current_measurement> [WHERE <stuff>] GROUP BY time(<interval>)[,<stuff>]
END
```

查询部分被 CREATE CONTINUOUS QUERY [...] BEGIN 和 END 所包含，主要的逻辑代码也是在这一部分。

```
CREATE CONTINUOUS QUERY sidekiq_gc_counts ON $DATABASE
BEGIN
    SELECT sum(count) AS total,
        sum(minor_gc_count) AS minor,
        sum(major_gc_count) AS major
    INTO downsampled.sidekiq_gc_counts
    FROM "$DEFAULT_RETENTION_POLICY".sidekiq_gc_statistics
    GROUP BY time(1m)
END

```

```sh
DROP CONTINUOUS QUERY <cq_name> ON <database_name>
```

[InfluxDb]: https://docs.influxdata.com/influxdb/v1.3/
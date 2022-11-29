---
title: postgresql
date: 2018-08-11 15:11:27
updated: 2018-08-11 15:11:27
tags:
  - psql
categories:
  - db
---

> 参考 https://www.cnblogs.com/z-sm/archive/2016/07/05/5644165.html
> [postgressql 官网](https://www.postgresql.org/docs/current/static/app-initdb.html)

# 简介

# mac 下安装与启动

1. 安装
```
brew install postgresql -v
```

运行完以上命令后 posgresql就被安装到了 `/usr/local/Cellar/postgresql`下了,并建立了一个链接目录`/usr/local/opt/postgresql`

- 默认的数据库路径为`/usr/local/val/postgres`
- 默认创建一个当前用户名同名的不带密码的数据库管理员账号
- 默认创建一个与当前用户名同名的数据库

2. 启动
```
brew services start postgres
 或者
pg_ctl -D /usr/local/var/postgres start
```

3. 停止
```
brew services stop postgres
 或者
pg_ctl -D /usr/local/var/postgres stop
```
4. 进去客户端
```
# 默认使用当前用户名，和当前用户名同名的数据库
psql
```

# ubuntu 下安装与启动

1. 安装
```
sudo apt-get install postgresql
```
安装成功后
- 默认创建名为 postgres 的 linux 用户
- 默认创建名为 postgres 不带密码的默认数据库管理员账号
- 创建名为 postgres 的表
- 配置目录为`/etc/postgresql/9.5/main`
- 默认数据库路径 `/var/lib/postgresql/9.5/main`
- 默认 socket 为 `/var/run/postgresql`
- 默认启动端口 5432

2. 启动与停止
```
sudo service postgresql start
sudo service postgresql stop
```

3. 进入客户端
```
# 以postgres用户身份运行psql命令
sudo -u postgres psql
```

# 添加新用户和新数据库
## 通过 psql 客户端添加
进入客户端
```
sudo -u postgres psql
```

创建用户 **exampleuser** 并设置密码
```
create user exampleuser whith password '123456';
```

创建数据库 **exampledb** ，所有者为 exampleuser
```
create database exampledb owner exampleuser;
```

将exampledb数据库所有权赋予exampleuser，否则exampleuser只能登陆不能操作
```
grant all privileges on database exampledb to exampleuser;
```

## 通过shell命令行添加
postgreql 安装后，提供 `createuser` 和 `createdb` 命令
```
# 以postgres用户身份运行createuser命令创建exampleuser，并指定为超级用户
sudo -u postgres createuser --superuser exampleuser
# 以postgres用户身份运行createdb命令创建exampledb数据库，并指定为所有者为exampleuser
sudo -u postgres createdb -O exampleuser exampledb
```

# psql

安装完后会有PostgreSQL的客户端psql
```
psql -U dbuser -d exampledb -h 127.0.0.1 -p 5432
```
- dbuser 数据库账号
- exampledb 数据库的名称
- 127.0.0.1 数据库的主机ip
- 5432 数据库的端口号

> 如果仅输入`psql`,相当于以当前用户名作为登陆账号，指定的数据库为与当前用户名同名的数据库，若不存在这两项则报错。

# 客户端操作命令

数据库的增删改查与SQL语法一致，以下是一些特定的语法

    # 设置密码
    \password
    # 退出客户端
    \q
    # 查看SQL命令解释，如 \h select
    \h <command>
    # 查看psql命令列表
    \?
    # 列出所有数据库，相当与show databases
    \l
    # 链接到其他数据库，相当于 use dbname
    \c <dbname>
    # 列出当前数据库的所有表, 相当于 show tables;
    \d
    # 列出某个表的结构，相当于 desc <table name>
    \d <table name>
    # 列出当前所有用户
    \du
    # 打开文本编辑器
    \e
    # 列出当前数据库和链接信息
    \conninfo

# initdb
postgresql 安装后提供 `initdb` 命令，可用于初始化一个数据库，而不使用默认的数据库

```
initdb --locale=C -E utf-8 <postgres_data_dir>
```

**postgres_data_dir**: 数据的保存路径，使用绝对路径

执行完以上命令后，在指定目录下就初始化了一个postgresql数据库，里面包括了配置文件等，与系统默认的postgresql独立开来。

待更新 。。。
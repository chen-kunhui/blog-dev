---
title: linux-ldap-install
mathjax: false
date: 2019-11-21 19:30:30
tags:
  - ldap
  - linux
  - ubuntu
categories:
  - linux
---

> 引用 https://blog.csdn.net/weixin_36485376/article/details/76737572

> https://help.ubuntu.com/lts/serverguide/openldap-server.html

> https://www.howtoing.com/how-to-install-and-configure-openldap-and-phpldapadmin-on-ubuntu-16-04

# ubuntu ldap 安装与启动

## 1.安装依赖

```
sudo apt install slapd ldap-utils
```

## 2.初始化配置

```
sudo dpkg-reconfigure slapd
```

> 在执行初始化配置时admin 密码最好和安装时输入的一样

## 3.启动 slapd

```
sudo service slapd start
```

## 4.测试服务是否能连接

```
ldapwhoami -H ldap:// -x
```

> anonymous是我们anonymous的结果，因为我们运行ldapwhoami而不登录到LDAP服务器。 这意味着服务器正在运行并应答查询

## 5. 创建一个 content.ldif 文件，添加如下内容

```
dn: ou=Users,dc=test,dc=com
objectClass: organizationalUnit
ou: Users

dn: ou=Groups,dc=test,dc=com
objectClass: organizationalUnit
ou: Groups

dn: cn=APP,ou=Groups,dc=test,dc=com
objectClass: posixGroup
cn: APP
gidNumber: 5000

dn: uid=zhangsan,ou=Users,dc=test,dc=com
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
uid: zhangsan
sn: zhangsan
givenName: zhangsan
cn: zhangsan
displayName: zhangsan
uidNumber: 10000
gidNumber: 5000
userPassword: Aa123456
gecos: zhangsan
loginShell: /bin/bash
homeDirectory: /home/zhangsan
```

## 6. 往ldap 服务中添加数据

```
ldapadd -x -D cn=admin,dc=test,dc=com -W -f content.ldif
```

## 7. 查询数据

```
ldapsearch -x -LLL -b dc=test,dc=com 'uid=zhangsan' cn gidNumber
```

输出内容如下，代表创建成功

```
dn: uid=zhangsan,ou=Users,dc=test,dc=com
cn: zhangsan
gidNumber: 5000
```


# 常用命令

## 服务启动与停止

```
sudo service slapd status|stop|start|restart
sudo /etc/ini.d/slapd start|stop|restart|status
```

# phpldapadmin

phpldapadmin 是用来管理 ldap 的一个web ui 界面

```
sudo apt-get install apache2
sudo apt-get install php libapache2-mod-php
sudo apt-get install phpldapadmin
sudo vim /etc/phpldapadmin/config.php
```

> phpldapadmin 配置文件 /etc/phpldapadmin/config.php

修改 `/etc/phpldapadmin/config.php` 
找到 `$servers->setValue('server','base',array('dc=example,dc=com'));`
将array('dc=example,dc=com') 修改为你的


在 /etc/apache2/apache2.conf 文件末尾追加 `ServerName 0.0.0.0`
修改 /etc/apache2/ports.conf 以修改监听的端口号

apache2 检查配置是否有错
```
apache2ctl configtest
```

管理 apache2 服务
```
service apache2 start|stop|restart|status
```


访问 phpldapadmin

```
http://ip:port/phpldapadmin/
```

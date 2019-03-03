---
title: "Centos7安装mysql的步骤"
date: 2018-10-06T12:25:52+08:00
draft: true
tags: ["mysql"]
series: ["数据库技术"]
categories: ["技术学习"]
toc: true
summary: "介绍在centos7中安装数据库的步骤和注意事项。"
---
## Centos7安装mysql的步骤

CentOS7默认数据库是mariadb,配置等用着不习惯,因此决定改成mysql,但是CentOS7的yum源中默认好像是没有mysql的。为了解决这个问题，我们要先下载mysql的repo源

### 1.下载mysql的repo源
```sh
$ wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```
### 2.安装*mysql-community-release-el7-5.noarch.rpm*包
```sh
$ sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
```
安装这个包后，会获得两个mysql的yum repo源：/etc/yum.repos.d/mysql-community.repo，/etc/yum.repos.d/mysql-community-source.repo。

### 3.安装*mysql*
```sh
$ sudo yum install mysql-server
```
根据提示安装就可以了,不过安装完成后没有密码,需要重置密码

### 4.重置*mysql*密码
```sh
$ mysql -u root
```
登录时有可能报这样的错：<font color=#00ff00 size=3>ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)</font>，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：

```sh
$ sudo chown -R root:root /var/lib/mysql
```
重启mysql服务

```sh
$ service mysqld restart
```
接下来登录重置密码：

```sh
$ mysql -u root  //直接回车进入mysql控制台 
mysql > use mysql; 
mysql > update user set password=password(‘123456’) where user=’root’; 
mysql> flush privileges; 
mysql > exit;
```


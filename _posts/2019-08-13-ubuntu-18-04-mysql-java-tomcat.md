---
layout: post
title: "Ubuntu 18.04 安装 MySQL, Java, Tomcat"
create: 2019-08-13
update: 2019-08-20
categories: blog
tags: [Ubuntu]
description: "How to Install MySQL, Java, Tomcat in Ubuntu 18.04"
---

## Prepare

```bash
sudo apt-get update
sudo apt-get upgrade
```

## Install Mysql

```bash
sudo apt-get install mysql-server
sudo mysql_secure_installation
```

## Install Java and Tomcat

```bash
sudo apt-get install default-jdk
tar -zxvf apache-tomcat-9.0.12.tar.gz
```

## Tomcat 开机自启动 rc.local for Ubuntu-18.04

```bash
sudo nano /etc/systemd/system/rc-local.service


[Unit]
Description=/etc/rc.local Compatibility
ConditionPathExists=/etc/rc.local

[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
StandardOutput=tty
RemainAfterExit=yes
SysVStartPriority=99

[Install]
WantedBy=multi-user.target


sudo nano /etc/rc.local


#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.
/home/admin/app/apache-tomcat-9.0.12/bin/catalina.sh start
exit 0


sudo chmod 755 /etc/rc.local
sudo systemctl enable rc-local
```

## 设置 Mysql 允许远程连接

```bash
mysql -uroot -p
use mysql
grant all privileges on *.* to 'root'@'%' identified by '$password';
flush privileges;
exit


重启mysql服务
sudo service mysql restart

如果仍然无法连接
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
找到
bind-address = 127.0.0.1
改为 
bind-address = 0.0.0.0

最后重启服务
sudo service mysql restart
```

## 可能会遇到的问题

### Mysql 只允许 root 登录

```bash
alter user 'root'@'localhost' identified with mysql_native_password by '$password';
```

### Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock'

```bash
sudo mkdir -p /var/run/mysqld
sudo chown mysql /var/run/mysqld/
sudo service mysql restart
```

### 如果仍然无法远程连接

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
找到 bind-address = 127.0.0.1 改为 bind-address = 0.0.0.0
sudo service mysql restart
```

### Mysql 时区不正确

```bash
set global time_zone = '+8:00';
flush privileges;

set persist time_zone='+0:00';
```

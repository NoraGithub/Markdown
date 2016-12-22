---
title: 在python里通过myql访问远程服务器数据库配置memo
tags:
  - mysql
  - python
  - ubuntu
  - mac
layout: post
keywords: '访问远程服务器数据库, mysqlclient, mysqlserver, python, mac, ubuntu '
date: 2016-12-21 21:35:51
updated:
---


# mac安装mysql

mac:`brew install mysql`

```
==> Caveats
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

To connect run:
    mysql -uroot

To have launchd start mysql now and restart at login:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  mysql.server start
==> Summary
🍺  /usr/local/Cellar/mysql/5.7.13: 13,344 files, 445.0M

```
ubuntu:sudo apt-get-install mysql-server mysql-client
sudo apt-get-install libmysqlclient-dev#  为了支持python，有依赖

[#reference-How to get the MySQL C API libraries on OS X](http://stackoverflow.com/questions/1857861/libmysqlclient15-dev-on-macs)
[mysql-server与mysql-client有什么区别？](http://bbs.csdn.net/topics/310117511)

[在 Mac 下用 Homebrew 安装 MySQL](http://blog.neten.de/posts/2014/01/27/install-mysql-using-homebrew/)
# python支持

mac:`pip install mysqlclient`/`easy_install mysql-python`
ubuntu:`pip install mysqlclient`

[#reference-MySQL database connector for Python](http://blog.csdn.net/a657941877/article/details/8944683)
[#reference-有哪些比较好的在 Python 中访问 MySQL 的类库？](https://www.zhihu.com/question/19869186/answer/98805967)

## 使用
import mysqldb


# 可能遇见的问题

## 连接远程服务器返回`ERROR 2003 (HY000): Can't connect to MySQL server`
原因就是Mysql数据库的默认配置文件my.cnf（linux下）中的bind-address默认为127.0.0.1，即只允许本机访问，需要取消该限制。

ubuntu下使用`locate my.cnf`获取文件地址，如果返回`locate: can not stat () `/var/lib/mlocate/mlocate.db':`，执行`updatedb`解决。
[locate: can not stat () `/var/lib/mlocate/mlocate.db': No such file or directory](http://blog.csdn.net/w13770269691/article/details/6987384)
```
root@localhost:~# locate my.cnf
/etc/alternatives/my.cnf
/etc/mysql/my.cnf
/etc/mysql/my.cnf.fallback
/var/lib/dpkg/alternatives/my.cnf
```
my.cnf一般在/etc/mysql下面，网络绝大部分资源都显示直接修改该文件。
root@localhost:~# cat /etc/alternatives/my.cnf
```
#
# The MySQL database server configuration file.
#
# You can copy this to one of:
# - "/etc/mysql/my.cnf" to set global options,
# - "~/.my.cnf" to set user-specific options.
# 
# One can use all long options that the program supports.
# Run program with --help to get a list of available options and with
# --print-defaults to see which it would actually understand and use.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

#
# * IMPORTANT: Additional settings that can override those from this file!
#   The files must end with '.cnf', otherwise they'll be ignored.
#

!includedir /etc/mysql/conf.d/
!includedir /etc/mysql/mysql.conf.d/
```
根据[Why /etc/mysql/my.cnf is EMPTY](http://askubuntu.com/questions/699903/why-etc-mysql-my-cnf-is-empty)，配置文件（.cnf文件，一般是mysqld.cnf）locate在
`/etc/mysql/conf.d/`和`/etc/mysql/mysql.conf.d/`中，我的mysqld.cnf地址是：`/etc/mysql/mysql.conf.d/mysqld.cnf`

这才开始修改我的配置（mysqld.cnf）
修改前的配置文件为：
```
#  
# Instead of skip-networking the default is now to listen only on  
# localhost which is more compatible and is not less secure.  
bind-address           = 127.0.0.1  
```
修改后：
```
#  
# Instead of skip-networking the default is now to listen only on  
# localhost which is more compatible and is not less secure.  
# bind-address           = 127.0.0.1  
```
修改后并未立即生效，需要重启生效（可以找办法restart该服务，我是直接reboot）

[#reference-远程连接Mysql数据库问题(ERROR 2003 (HY000)](http://tk-zhang.iteye.com/blog/735467)
[#reference-MySQL数据库无法远程连接的解决办法](http://www.jianshu.com/p/d501af0f127c)
[MySQL远程连接ERROR 2003 (HY000):Can't connect to MySQL server on'XXXXX'的问题](http://www.2cto.com/database/201204/127400.html)
[mac/ubuntu环境安装](https://ihower.tw/rails/advanced-installation.html)

## 连接远程服务器返回`ERROR 1130 (HY000): is not allowed to connect to this MySQL server`
服务器连接上mysql后， 授权myuser使用mypassword从任何主机连接到mysql服务器
`GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'192.168.1.3' IDENTIFIED BY 'mypassword' WITH GRANT OPTION; `

[mysql远程连接解决办法](http://www.cnblogs.com/chutianyao/archive/2011/12/28/2304332.html)
[无法远程登入MySQL Server解决](http://blog.csdn.net/rongjch/article/details/607124)
## ERROR 1045 (28000): Access denied for user
尝试从本地连接服务器：
`!mysql -h'104.128.92.224' -u<%username> -p<%password>`
反馈`mysql: [Warning] Using a password on the command line interface can be insecure.`
所以执行`mysql -h'104.128.92.224' -uroot`
但返回`ERROR 1045 (28000): Access denied for user`，属于粗心，正确的执行方式为：``mysql -h'104.128.92.224' -u<%username> -p`再输入密码即可。

[遇到问题---mysql账户密码以及权限的问题](http://blog.csdn.net/zzq900503/article/details/14163769)
[完整过程解决 ERROR 1045 (28000)](http://blog.csdn.net/nel0511/article/details/13091163)
[Ubuntu服务器常用配置－mysql数据库的安装](https://segmentfault.com/a/1190000002514402)

## log问题
[mysql错误日志分析](http://www.cnblogs.com/jackluo/archive/2013/03/04/2942500.html)
[IP address could not be resolved: Temporary failure in name resolution](http://blog.csdn.net/lxpbs8851/article/details/7892256)

## mysql连接到远程服务器的思考
mysql-client连接到mysql-server，通过什么协议发起请求？是否安全？看起来不是https 的
[Python and Connecting to MySQL over SSH](http://stackoverflow.com/questions/21386005/python-and-connecting-to-mysql-over-ssh)
[Connect on remote MySQL database through Python](http://stackoverflow.com/questions/12972867/connect-on-remote-mysql-database-through-python)
[python模块paramiko与ssh](http://python.jobbole.com/87088/)


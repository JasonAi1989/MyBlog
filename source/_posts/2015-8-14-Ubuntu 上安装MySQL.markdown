---
layout: post
title: Ubuntu 上安装MySQL
categories: 笔记
tags: Ubuntu Linux MySQL
---

###介绍

是一款免费的数据库程序，为Oracle公司旗下产品。

###安装MySQL

要安装 MySQL，可以在终端提示符后运行下列命令：

    sudo apt-get install mysql-server mysql-client #中途会让你输入一次root用户密码

注：如果在安装的过程中遇到问题，请看这篇文章[《安装MySQL失败解决方法》](http://www.jasonai.com/%E5%AE%89%E8%A3%85MySQL%E5%A4%B1%E8%B4%A5%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95/)

    sudo apt-get install php5-mysql  #安装php5-mysql 是将php和mysql连接起来

一旦安装完成，MySQL 服务器应该自动启动。

    sudo start mysql #手动的话这样启动

    sudo stop mysql #手动停止

当你修改了配置文件後，你需要重启 mysqld 才能使这些修改生效。

要想检查 mysqld 进程是否已经开启，可以使用下面的命令：

    pgrep mysqld

如果进程开启，这个命令将会返回该进程的 id。

###文件结构

MySQL配置文件：/etc/mysql/my.cnf ，其中指定了数据文件存放路径

    datadir         = /var/lib/mysql

如果你创建了一个名为 test 的数据库，那么这个数据库的数据会存放到 /var/lib/mysql/test 目录下。

###进入MySQL

    mysql -u root -p 

(输入mysql的root密码)

    $ mysql -u root -p
    Enter password: 
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 37
    Server version: 5.1.41-3ubuntu12.3 (Ubuntu)

    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

    mysql> 

修改 MySQL 的管理员密码：

    sudo mysqladmin -u root password newpassword；

###简单的操作

显示数据库：

    mysql> show databases;
    +--------------------+
    | Database           |
    +--------------------+
    | information_schema |
    | mysql              |
    +--------------------+
    2 rows in set (0.00 sec)

###设置远程访问

1.取消本地监听

正常情况下，mysql占用的3306端口只是在IP 127.0.0.1上监听，拒绝了其他IP的访问（通过netstat可以查看到）。取消本地监听需要修改 my.cnf 文件：

    sudo vim /etc/mysql/my.cnf
    //找到如下内容，并注释
    bind-address = 127.0.0.1

然后需要重启 mysql （可最后再重启）。

2.授权法

    mysql>GRANT ALL PRIVILEGES ON *.* TO <user>@"%" IDENTIFIED BY '<password>' WITH GRANT OPTION;
    mysql>FLUSH PRIVILEGES

第二句表示从mysql数据库的grant表中重新加载权限数据。因为MySQL把权限都放在了cache中，所以在做完更改后需要重新加载。

###phpmyadmin管理

用随便一个支持PHP的web服务器（如Apache、Nginx、Lighttpd），下载phpmyadmin，装之。

    sudo apt-get install phpmyadmin  #注意这是安装到/usr/share/phpmyadmin
---
title: Mysql5.7社区版安装教程
date: 2019-12-07 18:24:31
tags:
    - Mysql
categories: 安装教程
---

## 下载
[Mysql 5.7官网](https://dev.mysql.com/downloads/mysql/5.7.html)

[Mysql5.7.28 下载地址](https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.28-winx64.zip)

将下载社区版的mysql，解压以后进行安装。这里解压的目录为`D:\Program Files\mysql-5.7.25-winx64`

## 安装
1. 打开`cmd`通过命令添加环境变量：`setx PATH "%PATH%;D:\Program Files\mysql-5.7.25-winx64\bin"`
2. 然后重新打开`cmd`，执行`mysqld -install`进行安装mysql。
<!--more-->
- 执行`mysql -install`安装mysql
- 执行`mysql -remove`卸载mysql
```
注意：如果在这里报错提示缺少msvcr120.dll。
解决方案：
    下载并安装：Visual C++ Redistributable Packages for Visual Studio 2013
```
下载链接：[Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/zh-CN/download/details.aspx?id=40784)
3. 继续执行`mysqld --initialize`进行初始化。
4. 启动mysql服务，`net start mysql`
5. 连接mysql`mysql -u root -p`
6. 输入密码。在执行完初始化的时候，会在`D:\Program Files\mysql-5.7.25-winx64`目录下创建`data`文件夹，在`data`文件夹下找到`DESKTOP-EDAUTUO.err`。然后用文本文档打开，找到默认密码。文件内容如下：
```
2019-12-07T09:52:24.211349Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2019-12-07T09:52:24.707066Z 0 [Warning] InnoDB: New log files created, LSN=45790
2019-12-07T09:52:24.788368Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2019-12-07T09:52:24.924715Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: 4560590e-18d7-11ea-b76c-000c294577ac.
2019-12-07T09:52:24.939122Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2019-12-07T09:52:30.411768Z 0 [Warning] CA certificate ca.pem is self signed.
2019-12-07T09:52:31.196782Z 1 [Note] A temporary password is generated for root@localhost: Tyo58G.u?He#
```
通过文件，可以知道我的默认密码就是`Tyo58G.u?He#`。
7. 修改密码。输入刚刚的默认密码以后，回车就连接成功了mysql，但是为了方便我们下次的使用，在这里更改密码为`1234`，执行`ALTER USER 'root'@'localhost' IDENTIFIED BY '1234'`即可。

## 总结
以上安装过程在命令行如下：
```shell
C:\Windows\system32>setx PATH "%PATH%;D:\Program Files\mysql-5.7.25-winx64\bin"
```
在这里之所以重新打开`cmd`，是因为在设置环境变量以后，不重新打开`cmd`，`mysqld`的命令无法使用。
```shell
C:\Windows\system32>mysqld -install
Service successfully installed.

C:\Windows\system32>mysqld --initialize

C:\Windows\system32>net start mysql
MySQL 服务正在启动 .
MySQL 服务已经启动成功。


C:\Windows\system32>mysql -uroot -p
Enter password: ************
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.25

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'hervie';
Query OK, 0 rows affected (0.01 sec)

mysql> exit;

C:\Windows\system32>mysql -uroot -p1234
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.25

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```
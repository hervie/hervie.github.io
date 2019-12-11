---
title: Tomcat8安装教程
date: 2019-12-03 21:45:43
tags:
    - 安装教程
categories: Tomcat
---
[Tomcat官网](http://tomcat.apache.org/)

## 安装
将下载好的tomcat压缩文件解压到`D:\Programmer\apache-tomcat-8.5.42-windows-x64\apache-tomcat-8.5.42`

然后配置环境变量

| 变量名 | 变量值 |
| :---: | :--- |
| CATALINA_HOME | D:\Programmer\apache-tomcat-8.5.42-windows-x64\apache-tomcat-8.5.42 |
| PATH | %CATALINA_HOME%\lib |
| PATH | %CATALINA_HOME%\bin |

在`D:\Programmer\apache-tomcat-8.5.42-windows-x64\apache-tomcat-8.5.42\bin`下输入命令：`service.bat install`

得到以下结果
```shell
Installing the service 'Tomcat8' ...
Using CATALINA_HOME:    "D:\Programmer\apache-tomcat-8.5.42-windows-x64\apache-tomcat-8.5.42"
Using CATALINA_BASE:    "D:\Programmer\apache-tomcat-8.5.42-windows-x64\apache-tomcat-8.5.42"
Using JAVA_HOME:        "D:\Program Files\Java\jdk1.8.0_211"
Using JRE_HOME:         "D:\Program Files\Java\jdk1.8.0_211\jre"
Using JVM:              "D:\Program Files\Java\jdk1.8.0_211\jre\bin\server\jvm.dll"
The service 'Tomcat8' has been installed.
```
输入命令：`service.bat remove`可以移除注册服务

- 启动服务：net Start Tomcat8
- 关闭服务：net stop  Tomcat8

启动以后，浏览器打开`http://127.0.0.1:8080/`。就可以看见启动的Tomcat服务了。

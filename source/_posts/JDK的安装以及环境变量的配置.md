---
title: JDK的安装以及环境变量的配置
date: 2019-12-3 21:57:03
tags:
    - Java
categories: 安装教程
---
## 安装
首先将`jdk1.8.0_211`安装到`D:\Program Files\Java`。安装完成后，该文件下有两个文件夹，分别是`jdk1.8.0_211`和`jre1.8.0_211`。

## 环境变量配置

|变量名|变量值|
|-|-|
|JAVA_HOME|D:\Program Files\Java\jdk1.8.0_211|
|CLASSPATH|.;%JAVA_HOME%\lib\;%JAVA_HOME%\lib\tools.jar|
|PATH|%JAVA_HOME%\bin|
|PATH|%JAVA_HOME%\jre\bin|

**解决`javac -version`报错问题**：

PATH变量的`%JAVA_HOME%\bin`和`%JAVA_HOME%\jre\bin`分开，不是用**分号**隔开，而是如下方式：
```shell
%JAVA_HOME%\bin;%NVM_SYMLINK%;%JAVA_HOME%\jre\bin;
```
中间用其他的变量值隔开，即可解决`javac  -version`报错。
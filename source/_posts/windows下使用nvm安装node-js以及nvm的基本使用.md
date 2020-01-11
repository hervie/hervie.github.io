---
title: windows下使用nvm安装node.js以及nvm的基本使用
date: 2020-01-12 00:04:27
tags:
    - nvm
    - node
categories: 安装教程
---

# nvm安装

- nvm下载：[下载地址](https://github.com/coreybutler/nvm-windows/releases)
- 解压`nvm-setup.zip`，然后右键管理员权限运行安装。
- 添加 nvm 环境变量：`setx PATH "%PATH%;D:\Programmer\nvm"`
- 添加 node 环境变量：`setx PATH "%PATH%;D:\Programmer\nodejs"`（在安装的过程中，设置nodejs的文件夹为`D:\Programmer\nodejs`）

<!-- more -->
常用命令：
- `nvm install node`:安装最新的node.js
- `nvm install [version]`:安装指定版本的node.js
- `nvm use [version]`:使用某个版本的node.js
- `nvm list`:列出当前安装了哪些版本的node.js
- `nvm uninstall [version]`:卸载指定版本的node
- `nvm node_mirror [url]`:设置`nvm`镜像
- `nvm npm_mirror [url]`:设置`npm`镜像

设置国内镜像：
```shell
nvm node_mirror https://npm.taobao.org/mirrors/node/
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
```

# node安装

使用`nvm`安装`nodejs`：`nvm install 6.4.0`


# npm:
npm在安装node的时候就会自动安装了。前提条件是你需要设置node的版本号。

使用淘宝镜像:`npm install -g cnpm --registry=https://registry.npm.taobao.org`

## 安装包
安装包分为全局安装和本地安装。全局安装是安装在当前node环境中，在所有的项目中都可以使用这个包。而本地安装时安装在当前的项目中，只有在当前的项目中才可以使用。安装方式的区别是只有`-g`参数的区别：
```shell
npm install express
npm install express -g
```

---
title: Git快速入门
date: 2019-12-12 01:59:09
tags:
    - 版本控制
    - Github
categories: Git
---

# 安装Git

1. windows安装Git。首先[下载Git](https://git-scm.com/downloads)。

## 快速使用

### Git 创建仓库
- `git init`：用来初始化一个`Git`仓库
- `git init socket`：创建一个名为`socket`的初始化仓库

### git status
`git status`: 查看在你上次提交之后是否有修改

<!--more-->
例如
```shell
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        aliyunMessage.py

nothing added to commit but untracked files present (use "git add" to track)

```


### git add
`git add`: 将该文件添加到缓存

例如
```shell
$ git add aliyunMessage.py
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   aliyunMessage.py
```

### git commit
`git commit`: 将缓存区内容添加到仓库中。**在这之前需要配置`user.name`和`user.email`。**

```
$ git commit -m '阿里云发送短信验证码demo'
[master (root-commit) b3a5651] 阿里云发送短信验证码demo
 1 file changed, 40 insertions(+)
 create mode 100644 aliyunMessage.py
$ git status
On branch master
nothing to commit, working tree clean
```

### Git远程仓库（Github）
#### 添加远程库
**向GitHub提交代码**
```
git remote add [shortname] [url]
```

### 查看user和email
```git
git config --global user.name
git config --global user.email
```

### 修改user和email
```git
git config --global user.name 'hervie'
git config --global user.email '727634234@qq.com'
```


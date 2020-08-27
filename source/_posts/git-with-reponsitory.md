---
title: 使用Git管理自己的代码，将代码上传到Github(Windows)
thumbnail: https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/git-and-github.jpg
date: 2020-05-25 14:36:13
tags: git
categories: git
---

<div class='post-summary'>
    <p>
        将本地代码上传到Github做个备份，而且使用这个Git也可以控制代码版本。每次提交可以写上提交信息，比如更新了什么东西之类的，方便后续查看。
    </p>
</div>



<!--more-->

### 前提

#### 下载git

百度直接搜索`git`进入官网进行下载，然后安装。

#### 将本机与Github远程绑定

通过密钥进行绑定

### 创建Github仓库

在Github上创建一个新的仓库，并且`复制仓库地址`

### 创建本地仓库

在本地目录新建一个文件夹，这个就作为本地仓库了，安装好git之后，进入这个文件夹，右键选择Gitbash here然后输入下面命令来初始化这个git仓库：

```shell
git init
```

再配置提交者的用户名和邮箱：

```shell
git config user.name "用户名"
```

```shell
git config user.email "邮箱@gmail.com"
```

在这个仓库文件中随意存放或者新建一个文件，然后再gitbash中输入`git status`来查看还有什么文件是没有添加到提交目录中的（如果文件显示红色，则这个文件还未被添加到待提交的目录）：

```shell
git status
```

将待提交的文件加入到提交目录：

```shell
git add 文件
```

给这个本地仓库添加远程地址：

```shell
git remote add origin 远程地址
```

提交并且输入提交信息：

```shell
git commit -m "first commit"
```

将待推送的文件推送到Github主分支：

```shell
git push origin master
```


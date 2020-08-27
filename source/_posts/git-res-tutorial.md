---
title: 使用GitHub仓库来保存自己的代码
date: 2020-04-07 16:35:16
tags: Github
categories: Github
---
### 绑定GitHub
#### 获取电脑的密钥
+ 在Gitbash中输入<span style='color: red;font-weight: bold;'>`Add-AppxPackage -register AppxManifest.xml`</span>并回车，回车，再回车
+ 打开用户目录，找到`.ssh\id_rsa.pub`文件，记事本打开复制里面的内容，这就是密钥
+ 点击Github自己的头像里面的`setting`，找到`SSH and GPG keys`，点击右侧`New SSH key`随意填一个名称，在下面填入`id_rsa.pub`里面的内容，保存，这样就把电脑和Github绑定了
<!--more-->
#### 本地上传到GitHub
+ GitHub里面新建一个代码仓库，名称随意
+ 进入仓库,复生成的https链接：

<div align='center'>
	<img src='https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/git.png'>
</div>


+ 在本地新建一个文件夹，使用`GitBash`进入这个文件夹，执行命令：
```
    git init
```
+ 命令执行完毕后这个文件夹下面就会生成一个.git的文件夹，这样就初始化完毕了
+ 在文件夹下新建一个helloGit.txt文件做测试
+ 执行以下命令
```
    git add helloGit.txt // 选择文件，加入提交暂存区
    git commit -m "提交测试" // 本次提交的描述信息
    git remote add origin 刚刚复制的链接 // 绑定刚刚创建的远程Git仓库
    git push -u origin master // 提交
```
这样就完成了将自己电脑和GitHub远程仓库的绑定



---
title: 使用Express和express-art-template来编写Node(配合nodemon)
thumbnail: https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/express.jpg
date: 2020-05-13 10:44:18
tags: 笔记
categories: node
---

<div class='post-summary notification is-warning'>
    使用Express来开发node可以使开发变得很方便，可以抽离路由从而让每个请求处理变得很清晰，加上express-art-template模板引擎让页面渲染数据变得很简单。使用nodemon则不用去频繁的重复开启关闭服务器，nodemon会监听服务器脚本文件，一旦有变化会自动重启服务器。
</div>

<!--more-->

### Express

**安装：**

```shell
npm install --save express
```

**使用express-generator来自动创建express项目**：

```shell
npm install -g express-generator
```

**创建express项目：**

```shell
express myExpress  // 会创建一个名为myExpress的项目目录
```

**项目目录：**

```
.
├── app.js    //服务器文件
├── bin
│   └── www
├── package.json  // 资源包、开发依赖
├── public       // 公共资源目录，可以自由访问，没有限制
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes    // 路由目录
│   ├── index.js
│   └── users.js
└── views    // 视图目录
    ├── error.pug
    ├── index.pug
    └── layout.pug
```



### express-art-template

**安装：**

```shell
npm install --save art-template express-art-template
```

**配置app.js，新增一条规则：**

```javascript
app.engine('html', require('express-art-template'));
```

**使用：**

```javascript
app.get('/', function(request, response){
    // express默认会去项目的 views 目录中找 index.html
    response.render('index.html', {
        title: 'Hello World'
    })
})
```

### nodemon

**安装：**

```shell
npm install -g nodemon
```

**使用：**

```shell
nodemon app.js // 使用 nodemon 命令来代替 node 去执行服务器文件
```

Over,enjoy......

<style>
    .post-summary{
        display: none;
    }
</style>


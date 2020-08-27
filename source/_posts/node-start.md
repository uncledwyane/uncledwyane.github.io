---
title: 【新坑】学习node.js
thumbnail: https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/node.jpg
date: 2020-05-09 18:03:54
tags: node
categories: node
---

<div class='notification is-danger post-summary'>
    <p>
        学习了vue之后想做一个类似于后台管理系统这样的项目，网上搜索了一圈，需要用到node的express来开发，正好应该学一学后端。
    </p>
</div>

<!--more-->

### 介绍

**根据官网介绍来看：**

- 简单的说 Node.js 就是运行在服务端的 JavaScript。
- Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。
- Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

### 文件操作

**先引入node中的fs文件模块:**

```javascript
var fs = require('fs')
```

#### 读文件

```javascript
fs.readFile('test_read.txt', function(err, data){
    if(err)
        console.log("读取失败，错误为：" + err)
    console.log("读取成功，内容：" + data.toString())
});
```

#### 写文件

```javascript
fs.writeFile('test_write.txt', function(err){
    if(err)
        console.log("写入失败，错误为：" + err)
    console.log("写入成功")
});
```

### 创建一个本地服务器

**引入node中的http模块**

```javascript
var http = require('http')
```

#### 创建服务

```javascript
var server = http.createServer()
```

#### 开启服务

```javascript
server.on('request', function(request, response){ // 回调函数里有两个参数，第一个为请求，第二个是响应
    console.log("服务器开启成功，输入http://localhost:3000即可访问")
    console.log("请求的路径是：" + request.url)
    console.log()
})



```

#### 使用自定义端口

```javascript
server.listhen(3000， function(){
    console.log("服务启动成功，可以访问")
})
```











<style>
    .post-summary{
        display: none;
    }
</style>


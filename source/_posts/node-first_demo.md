---
title: Node小案例 简易评论系统
date: 2020-05-11 19:21:50
tags: 项目
categories: node
---

<div class='post-summary notification is-success'>
    <p>
       学习两天Node然后做的一个非常简易的评论系统，涉及到请求处理，请求重定向，使用art-template模板引擎渲染等。
    </p>
    <p>
        涉及到的Node中的模块：
    </p>
    <ul>
        <li>http</li>
		<li>url</li>
        <li>fs</li>
        <li>art-template（第三方）</li>
    </ul>
</div>


<!--more-->

### 项目演示

<div class='images' align='center'>
    <img src='https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/test.gif' alt='项目动图演示'/>
</div>



### 代码

**服务器脚本文件app.js**

```javascript
var http = require('http')
var fs = require('fs')
var template = require('art-template')
var url = require('url')

var comments = [
    {
      name: '张三',
      message: '今天天气不错！',
      dateTime: '2020-4-15'
    },
    {
      name: '韦德',
      message: 'This is my house!',
      dateTime: '2013-5-9'
    }
]

http
    .createServer(function(request, response){
        var parseObj = url.parse(request.url, true)
        var pathname = parseObj.pathname
        if(pathname === '/'){
            fs.readFile('./views/index.html', function(err, data){
                if (err) {
                    fs.readFile('./views/404.html', function(err, data){
                        response.end(data)
                    })
                }
                else{
                    // 模板引擎渲染
                    var res = template.render(data.toString(), {
                        comments: comments
                    })
                    response.end(res)
                }
            })
        }
        else if(pathname === '/post'){
            fs.readFile('.' + url, function(err, data){
                if(err){
                    fs.readFile('./views/post.html', function(err, data){
                        if (err) {
                            fs.readFile('./views/404.html', function(err, data){
                                response.end(data)
                            })
                        }
                        else{
                            response.end(data)
                        }
                    })
                }
                else{
                    response.end(data)
                }
            })
        }
        else if(pathname === '/pinglun'){
            // 设置请求头
            response.setHeader('Content-Type', 'text/plain;charset=utf-8')
            // 获取使用url模块解析过的传递的数据
            var comment = parseObj.query
            // 格式化时间
            var date = new Date()
            var year = date.getFullYear()
            var month = date.getMonth() + 1
            var day = date.getDay()
            var hour = date.getHours()
            var minutes = date.getMinutes()
            var seconds = date.getSeconds()
            // 将时间添加到评论的时间
            comment.dateTime = year + '-' + month + '-' + day + ' ' + hour + ':' + minutes + ':' + seconds
            // 将新的评论追加到评论列表
            comments.push(comment)
            // 设置状态码
            response.statusCode = 302
            // 重定向，重新请求首页地址
            response.setHeader('Location', '/')
            // 返回资源
            response.end()
            
        }
        else if(pathname.indexOf('/public/' === 0)){
            fs.readFile('.' + pathname, function(err, data){
                if(err){
                    fs.readFile('./views/404.html', function(err, data){
                        response.end(data)
                    })
                }
                else{
                    response.end(data)
                }
            })
        
        }else{
            fs.readFile('./views/404.html', function(err, data){
                response.end(data)
            })
        }
    }).listen(3000, function(){
        console.log('Server is running......')
    })
```

**首页评论index.html**

```html
<body>
  <div class="header container">
    <div class="page-header">
      <h1>留言板<small>test</small></h1>
      <a class="btn btn-primary" href="/post">发表留言</a>
    </div>
  </div>
  <div class="comments container">
    <ul class="list-group">
      {{each comments}}
      <li class="list-group-item">{{ $value.name }}说：{{ $value.message }} <span class="pull-right">{{ $value.dateTime }}</span></li>
      {{/each}}
    </ul>
  </div>
</body>
```

**发表评论post.html**

```html
<div class="header container">
    <div class="page-header">
      <h1><a href="/">首页</a> <small>发表评论</small></h1>
    </div>
  </div>
  <div class="comments container">
    <form action="/pinglun" method="get">
      <div class="form-group">
        <label for="input_name">你的大名</label>
        <input type="text" class="form-control" required minlength="2" maxlength="10" id="input_name" name="name" placeholder="请写入你的姓名">
      </div>
      <div class="form-group">
        <label for="textarea_message">留言内容</label>
        <textarea class="form-control" name="message" id="textarea_message" cols="30" rows="10" required minlength="5" maxlength="20"></textarea>
      </div>
      <button type="submit" class="btn btn-primary">发表</button>
    </form>
  </div>
```



<style>
    .post-summary{
        display: none;
    }
</style>





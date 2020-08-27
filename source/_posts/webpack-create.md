---
title: 开始学习webpack啦！ 使用webpack处理一个简易的项目
date: 2020-03-27 18:38:50
tags: webpack
categories: webpack
thumbnail: https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/webpack.jpg

---
这只是开始学习webpack的第一步，确是我人生的一大步。
**介绍：**webpack 是前端的一个项目构建工具，它是基于 Node.js 开发出来的一个前端工具；Webpack是基于整个项目进行构建的；借助于webpack这个前端自动化构建工具，可以完美实现资源的合并、打包、压缩、混淆等诸多功能。
<!--more-->

### **准备工作**
1. 新建一个项目文件夹
2. 在这个项目文件夹下新建名为dist、src的文件夹
3. 在src文件夹下新建名为css、images、js的三个文件夹
4. 在src文件夹下新建名为index.html和main.js的文件
5. 上述步骤完成之后，在编辑器终端中进入这个项目文件夹，依次执行以下命令
- `npm init` //初始化
- `npm install webpack --save-dev` // 安装webpack

---
<div align='center'>
	<img src='https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/webpack_demo.jpg' alt='项目结构'>
</div>

---

#### **项目实例**
##### index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Document</title>
    <script src="../src/main.js"></script>
</head>
<body>
    <ul>
        <li>Hello Vue 1</li>
        <li>Hello Vue 2</li>
        <li>Hello Vue 3</li>
        <li>Hello Vue 4</li>
    </ul>
</body>
</html>
```
##### main.js
```javascript
// 这个例子是安装了jquery之后
import $ from 'jquery'
$(function (){
    $('li:odd').css('backgroundColor', 'lightred')
    $('li:even').css('backgroundColor', 'lightpink')
})
```
当index.html和main.js写好之后，直接打开index.html是看不到效果的，srcipt标签引用的main.js不能直接被浏览器解析，第一行**`import $ from 'jquery'`**就属于es6的高级语法，浏览器不能解析。
这样就开始webpack的第一次使用，在终端运行**`webpack ./src/main.js -o ./dist/bundle.js --mode=none`**,等待执行完毕

---
<div align='center'>
	<img src='https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/webpack_rezult1.jpg' alt='执行成功'>
</div>

- - -

Asset就为webpack处理后的js文件，这样一来，修改script标签中的src为`src='../dist/bundle.js'`就可以看到页面成功渲染为main.js中的样式

**问题来了：**每次编译都要输入`webpack ./src/main.js -o ./dist/bundle.js --mode=none`很麻烦，怎么变简单一点？
**解决：** 配置webpack配置文件，在项目根目录新建一个名为`webpack.config.js`的文件,设置简易配置:
`entry: 入口文件，要被webpack处理的文件`
`output: 出口，webpack处理后的放置文件的目录和文件名（默认为bundle.js）`

---
```javascript
	const path = require('path')
    module.exports = {
    	mode: 'none',
        entry: path.join(__dirname, './src/main.js'),
        output: {
        	path: path.join(__dirname, './dist'),
            filename: 'bundle.js'
        }
    }
```

---

这样一来，直接在终端运行webpack即可使用webpack处理

有新的问题： 开发中修改代码是常事，每次修改了都要输入webpack命令然后打开网页刷新，很麻烦，怎么修改之后自动编译并且刷新页面
**解决：**使用webpack-dev-server来自动化处理
**如果运行npm run dev出现can't find moudle**这种错误，删除掉项目中的node_modules目录重新在终端执行`npm install`即可

- 安装：`npm install webpack-dev-server -D`
- 设置项目的package.json，在scripts中添加一条：`"dev": "webpack-dev-server"`并保存
- 项目终端运行`npm run dev`就可以运行,输出的log中的http://localhost:8080/即为项目的本地服务器地址
- 修改script标签中的src为`src='/bundle.js'`，再点击本地服务器地址，点击页面中的src即可看见index.html并且已被正确渲染
- 现在只要main.js被修改并且保存了就会触发更新，自动刷新页面

- - -

<div align='center'>
	<img src='https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/webpack_dev_server.jpg' alt='运行成功'>
</div>

- - -





























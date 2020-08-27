---
title: webpack项目的创建及初始化(优化版)
date: 2020-04-27 1:33:11
tags: webpack
toc: true
categories: webpack
---
<div class="post-summary">
    这个webpack项目的创建是在之前的那篇基础之上优化而来，之前因为刚接触，逻辑不清晰。在做了一个小项目之后对webpack的创建以及工作原理有了更深的认识，便有了这篇博客。
</div>



<!--more-->

### **步骤：**

<article class="message is-danger">
   	<div class="message-body">
        <ol>
            <li>创建项目根目录Demo</li>
            <li>在Demo目录下创建dist、src目录</li>
            <li>在src目录下创建css、images、js文件夹</li>
            <li>在src目录下创建main.js、index.html文件</li>
            <li>在Demo根目录下创建webpack.config.js文件</li>
        </ol>
    </div>
</article>

### **文件结构图**

```
|-- Demo
    |-- webpack.config.js // webpack配置文件，配置各种loader和plugin等
    |-- .babelrc  // babel配置
    |-- dist // 打包后的存放目录
    |-- src // 资源文件夹吧
        |-- index.html   // 项目入口
        |-- main.js 	 // 导入各种包的文件
        |-- css 	 // 存放样式的目录
        |-- images	 // 存放图片资源的目录
        |-- js           // 存放其他脚本文件
```

### **准备工作完了，开始创建**

<article class="message is-success">
   	<div class="message-body">
        <ul>
            <li>使用`npm install webpack --save-dev`以依赖方式安装webpack</li>
            <li>使用`npm install webpack-cli --save-dev `安装webpack脚手架</li>
            <li>使用`npm install webpack-dev-server --save-dev`安装webpack工具，这样可以自动打包项目并更新</li>
        </ul>
    </div>
</article>

### **测试**

编辑`webpack.config.js`配置文件如下：

``````javascript
const path = require('path'); 
module.exports = {
    entry: './src/man.js', // 入口，需要被webpack编译打包的文件
    output: { 	// 出口
        path: path.resolve(__dirname, './dist'), // 打包出口路径
        filename: 'bundle.js' // 打包后的文件名
    }
}
``````

在`package.json`文件中的scripts下新建一条规则：

``````json
"dev": "webpack-dev-server --open --contentBase src --hot"

--open 是自动打开项目地址
--contentBase 参数是src，默认打开项目地址中的src，
也就是启动项目会自动打开index.html这个项目入口
--hot 热更新，在代码中更改了页面中的某些样式的时候，
页面不用刷新也能看到样式的改变
``````

+ 使用`npm install jquery --save-dev`安装jquery做一下测试

安装完成jQuery之后，在`main.js`写几条语句测试：

```javascript
import $ from 'jquery';

$(function(){
    $('#app').css('color','red');
})
```

在`index.html`中添加一个id为app的div，并引入bundle.js

```html
<script src='/bundlr.js'></script>

<div id='app'>
</div>
```



<div class="notification is-info">现在所有基本工作已结束，使用`npm run dev`运行项目，运行成功没有报错的话会在默认浏览器中自动打开项目的src中的index.html并且已经成功看到id为app的div中的字体颜色为<span class='warning'>红色</span>了。</div>

### **完善项目配置**

在上述步骤中，其实还没发挥webpack的真正实力，现在通过对webpack.config.js进行配置，逐步来实现webpack更强大的功能。

借参考资料上面的说明：在webpack的世界里，每个文件都是一个模块，比如.css、.js、.html、.less等。对于不同的模块，webpack不能直接识别，所以就需要不同的`加载器(loader)`来处理，而`加载器`就是webpack最重要的功能。通过安装不同的加载器可以对各种后缀名的文件进行处理。接下来就是一些loader的使用

### 配置loader

#### 针对css

```npm install css-loader style-loader  ```

安装完成在`webpack.config.js`中的module的rules中新增一条规则如下：

```javascript
{
    test: /\.css$/,
    use: ['style-loader', 'css-loader']
}
```

#### 针对vue

```npm install vue-loader``` 

安装完成在`webpack.config.js`中的module的rules中新增一条规则如下：

```javascript
{
    test: /\.vue$/,
    use: 'vue-loader'
}
```

同时`vue`需要和`VueLoaderPlugin`配合使用才能生效，所以还需要配置`webpack.config.js`:

```javascript
const VueLoaderPlugin = require('vue-loader/lib/plugin');

// 然后在增加一条plugins

module.exports = {
    ...,
    plugins: [
    	new VueLoaderPlugin()
    ]
}
```

<style type='text/css'>
    .post-summary{
        display: none;
    }
    .warning{
        background-color: red;
        color: #fff;
        padding: 4px;
        font-weight: bolder;
        border-radius: 5px;
    }
</style>
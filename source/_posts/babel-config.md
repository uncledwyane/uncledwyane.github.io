---
title: webpack中配置babel（babel7.X）
thumbnail: https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/babel.png
date: 2020-04-29 22:25:30
tags: [webpack,babel]
categories: webpack
---

<div class='post-summary'>
    <div class='notification is-info'>
        <p>
            babel是什么？
        </p>
        <p>
            babel的作用是把 JavaScript 中 es2015/2016/2017/2046 的新语法转化为 es5，让低端运行环境(如浏览器和 node )能够认识并执行。就是让以前的浏览器也能识别新的es6的语法。
        </p>
    </div>
</div>



<!--more-->

### 下载插件

#### 使用npm进行下载如下插件

+ @babel/core
+ @babel/plugin-proposal-class-properties
+ @babel/plugin-transform-runtime
+ @babel/preset-env
+ @babel/runtime
+ babel-loader

### 配置

<div class='notification is-success'>
    在项目根目录新建一个名为`.babelrc`的babel配置文件，配置如下：
</div>

```json
{
    "presets": ["@babel/preset-env"],
    "plugins": [
        "@babel/plugin-transform-runtime",
        "@babel/plugin-proposal-class-properties"
    ]
}
```

<div class='notification is-success'>
    在`webpack.config.js`中增加对js文件的配置（配置完成就可以在项目中使用es6语法而不用担心兼容性的问题了）：
</div>

```javascript
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
module.exports = {
    entry: './src/main.js',
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                use: 'vue-loader'
            },
            {
                test: /\.css$/,
                use: ['style-loader', 'css-loader']
            },
            {
                test: /\.(ttf|svg|woff|png|jpg|gif|jpeg)$/,
                use: 'url-loader'
            },
            {
                test: /\.js$/,
                use: 'babel-loader',  // 配置的babel
                exclude: /node_modules/
            }
        ]
    },
    plugins: [
        new VueLoaderPlugin()
    ]
}
```



<style>
    .post-summary{
        display: none;
    }
</style>


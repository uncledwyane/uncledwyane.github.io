---
title: webpack中的各种loader的安装及使用案例
thumbnail: https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/howtouninstall.png
date: 2020-03-29 15:14:46
tags: webpack
categories: webpack

---
webpack在不使用loader的情况下很多语句都不能被解析，所以需要不同的第三方loader来解析不能被webpack解析的语句。
<!--more-->

## **css loader**
安装： 
> npm install style-loader css-loader

配置（在`webpack.config.js`的`module`中新建一条rules）：
> test: /\.css$/, use: ['style-loader', 'css-loader']

## **url loader**
安装（file-loader为url-loader的内部依赖，在规则里可以不用写url-loader）： 
> npm install url-loader file-loader 

配置（在`webpack.config.js`的`module`中新建一条rules,这里以图片举例）：
> test: /\.(jpg|png|gif|bmp|jpeg)$/, use: ['url-loader?limit=7000']  //针对图片
> test: /\.(ttf|eot|svg|woff|woff2)$/, use: ['url-loader']   //针对字体

给url-loader添加参数，不让所有的图片都被解析为Base64格式，上面的`?limit=7000`意思是如果图片大小等于或者大于7000字节则图片不会被转为Base64编码格式（limit单位为byte），如果小于，则会被转为Base64格式。

## **在webpack中使用 vue 开发**
> npm install vue -D

配置webpack.config.js（在resolve中的alias中添加一条规则）：
```javascript
resolve: {
	alias: {
    	"vue": "vue/dist/vue.js"
    }
}
```
在main.js中添加一条语句
```javascript
import Vue from "vue"
```

## **vue-loader**
在webpack中，推荐使用 `.vue` 这个组件模板文件定义组件，所以需要安装 `vue-loader` 
> npm install vue-loader vue-template-compiler -D

## 上述vue-loader安装完成后，就可以使用webpack来创建一个组件并用render函数来渲染到页面上
1. 在js目录中新建一个login.vue文件，内容如下（vue模板，包含三部分）：
```
<template>
	<div>
    	<h1>这是登录组件</h1>
    </div>
</template>
<script>
</script>
<style>
</style>
```

2. 在main.js中导入这个组件，导入vue，并且new一个vue实例，并注册从login.vue导入的组件
```javascipt
import Vue from 'vue'
import login from './src/js/login.vue'
var app = new Vue({
	el:'#app',
    components: {
    	'login': login
    },
    //原始版：
    render: function(createElement){
    	return createElement(login)
    }
    //简写版：
    render: c => c(login)
})
```

3. 最后在index.html中创建一个id为app的div元素，作为app的实例要控制的区域
```html
<div id='app'>
	<login></login>
</div>
```





























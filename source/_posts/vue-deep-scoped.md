---
title: Vue组件中css不能更改v-html内的样式的解决办法
date: 2020-04-12 12:43:51
tags: Vue笔记
categories: Vue.js
---
今天在项目中使用axios请求数据的时候，返回的内容里含有html的各种标签，使用v-html渲染到组件页面的时候发现在style里直接更改v-html内的元素的样式没有任何效果，但是删除style的scoped属性之后，就有效果了，但是这种会引起问题，scoped对渲染的范围作了限制，去掉就没有限制。
针对这个问题，搜了一下，发现可以使用`deep scoped`来更改v-html内的样式
<!--more-->
使用方法(举例一个div, 假设v-html中有一个类名称为content)：

```html
<div class='container'>
	<p v-html='content'></p>
</div>
```
渲染：
```css
	.container >>> .content{
    	background-color: red;
        width: 300px;
    }
```

现在v-html中的.content就会有指定的样式了
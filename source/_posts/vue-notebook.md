---
title: Vue-笔记
date: 2020-02-11 14:58:10
tags: Vue
categories: Vue.js
---

<center>在Vue学习中的笔记</center>
<!--more-->

笔记1：

```java
var _this = this; //这句话可以用于防止直接用this导致指向不明>
```
<h5 align="left">v-cloak解决闪动问题</h5>
<p>通过给涉及到的元素加上一个v-cloak来解决</p>
```css
[v-cloak]{
	display: none;
}
```
```html
<div id="app" v-cloak>
	<p>{{ msg }}</p>
	<my-com>
		<template slot-scope='props'>
			<p>{{ props.msg }}</p>
		</template>
	</my-com>
</div>
```

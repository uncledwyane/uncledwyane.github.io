---
title: Vue mixins的厉害
date: 2020-02-28 17:40:35
tags: Vue
categories: Vue.js
---
<h4 align="left">mixins:混合</h4>
<p align="left">用处：当有多个组件使用了相同方法或变量，就可以用mixins来把相同的变量或方法封装到一个对象里，哪个组件需要就可以使用mixins: [mixins对象名]来调用啦</p>
<p align="left">一处定义，多处复用</p>
<div align="center">
	<img src="https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/vue_mixins.gif" alt="效果">
</div>
<!--more-->

---
<h4 align="center">使用了mixins</h4>
```html
	<div id="app">
		<popup></popup>
		<toggle></toggle>
	</div>
	<!---->
	<template id='popup'>
		<div>
			<h1 @mouseenter='show' @mouseleave='hide'>HeiHei</h1>
			<p v-if='visible' class="para">This is hide COntent</p>
		</div>
	</template>
	<template id='toggle'>
		<div>
			<button @click='isVisible'>Visible</button>
			<p v-if='visible'>Lorem ipsum dolor sit amet consectetur adipisicing elit. Tempora itaque nesciunt voluptates, facilis corrupti quisquam maxime numquam odio nihil amet dolorum praesentium quibusdam dolor odit qui a, fuga quod soluta?</p>
		</div>
	</template>
	<script src="lib/vue.js"></script>
	<script>
		//声明一个名为base的对象，用来装会被复用的变量和方法，声明一次，多处使用
		var base = {
			data: function (){
				return {
					visible: false
				}
			},
			methods: {
				show:function (){
					this.visible = true;
				},
				hide: function (){
					this.visible = false;
				},
				isVisible: function (){
					this.visible = !this.visible;
				}
			}
		};
		Vue.component('popup', {
			template: '#popup',
			//调用base对象，直接调用base内的变量和方法
			mixins: [base]
		})
		Vue.component('toggle', {
			template: '#toggle',
			//调用base对象，直接调用base内的变量和方法
			mixins: [base]
		});
		new Vue({
			el: '#app'
		})
	</script>
```
---
<h4 align="center">没有使用mixins</h4>
```html
<script>
		Vue.component('popup', {
			template: '#popup',
			data: function (){
				return {
					visible: false
				}
			},
			methods: {
				show:function (){
					this.visible = true;
				},
				hide: function (){
					this.visible = false;
				}
			}
		})
		Vue.component('toggle', {
			template: '#toggle',
			data: function (){
				return {
					visible: false
				}
			},
			methods: {
				isVisible: function (){
					this.visible = !this.visible;
				}
			}
		});
		new Vue({
			el: '#app'
		})
	</script>
```
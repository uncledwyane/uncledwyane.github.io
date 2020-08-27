---
title: Vue 组件通信 | 平行组件的通信
date: 2020-02-22 17:27:59
tags: Vue
categories: Vue.js
---
<center>一个兄弟组件之间的通信</center>
<!--more-->

---

<div align="center">
	<img src="https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/vue-component1.gif" alt="效果图">
</div>

---

<div align="center">
	<img src="https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/vue-component2.png" alt="元素结构图">
</div>

---

>代码中有两个平行组件ming,huahua，ming组件的内容可以传到huahua组件，huahua可以获取ming传输的内容
>ming组件内通过给input元素绑定监听事件keyup来获取用户状态，当用户在input里输入内容，就会触发$emit调用on_change()函数，发送this.i_said的数据，再在huahua的组件中通过$on来监听$emit传出的数据，并使用回调函数把获取到的data赋值给huahua_said，就完成了平行组件之间的通信
>关键点： 在JavaScript里第一行声明了一个名为Event的空Vue实例，作为一个中间量，用来让两个平行组件通信；

---
<h3 align="center">下面为代码</h3>
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title>Document</title>
	<link rel="stylesheet" href="">
	<style media="screen">
		#app{
			width: 230px;
			height: 65px;
			box-shadow: 0 0 30px rgba(0, 0, 0, 0.1);
			border-radius: 10px;
			font-family: 'XHei';
			padding: 20px;
		}
	</style>
</head>
<body>
	<div id="app">
		<ming style="margin-bottom: 20px	"></ming>
		<huahua></huahua>
	</div>
	<script src="lib/vue.js" charset="utf-8"></script>
	<script type="text/javascript">
		//这里的Event很关键，声明一个名为Event的空Vue实例，作为一个中间量，用来让两个平行组件通信；
		var Event = new Vue();
		Vue.component('ming',{
			template: `
			<div>
				我说：<input @keyup='on_change()' v-model='i_said' type="text" />
			</div>
			`,
			data: function (){
				return {
					i_said: ''
				}
			},
			methods: {
				//当用户在input中输入内容，就会触发i_said_something事件，并将i_said的数据发送出去
				on_change: function (){
					Event.$emit('i_said_something', this.i_said)
				}
			}
		});
		Vue.component('huahua', {
			template: `
			<div>
				花花说： {{ huahua_said }}
			</div>
			`,
			data: function (){
				return {
					huahua_said: ''
				}
			},
			//在生命周期mounted钩子函数里监听了来自Event的时间i_said_something
			mounted: function (){
				var _this = this;
				Event.$on('i_said_something', function (data){
					_this.huahua_said = data;
				})
			}
		})
		new Vue({
			el: '#app'
		});
	</script>
</body>
</html>

```
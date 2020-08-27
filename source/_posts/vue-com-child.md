---
title: Vue 组件 | 子组件索引
date: 2020-02-28 16:13:13
tags: Vue
categories: Vue.js
---
<h4 style="color: #999;" align="left">Vue提供的子组件索引方法</h4>
<p align="left">使用特殊的属性ref来为子组件指定一个索引名称</p>
<p align="left">在父组件模板中，子组件标签上使用了ref指定了一个名称ComA，并在父组件内通过this.$refs来访问ComA这个子组件</p>
<div align="center">
	<img src="https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/vue_com_child.gif" alt="Demo">
</div>

<!--more-->

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
</head>
<body>
	<div id="app">
		<button @click="handleRef">通过ref获取子组件实例</button>
		<my-item ref="comA"></my-item>
	</div>
	<script src="lib/vue.js"></script>
	<script>
		Vue.component('my-item', {
			template: "<div>子组件</div>",
			data:  function (){
				return {
					message: '子组件内容'
				}
			}
		});
		var app = new Vue({
			el: '#app',
			methods: {
				handleRef: function (){
					var msg = this.$refs.comA.message;
					console.log(msg);
				}
			}
		})
	</script>
</body>
</html>
```
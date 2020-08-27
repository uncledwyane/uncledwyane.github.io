---
title: Vue 组件 | 父子组件通信
date: 2020-02-18 17:30:05
tags: Vue
categories: Vue.js
---
<center>组件通信-父子组件的通信</center>
<!--more-->


> 文中涉及的$emit()解释：
用法： $emit( eventName, […args] )
eventName:事件名，会绑定一个方法。当组件触发事件后，将调用这个方法。
...args: 附加参数，会被抛出，由上述绑定的方法接收使用。

```html
<!DOCTYPE html>
<html lang="zh">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title></title>
</head>
<body>
	
	<div id="app">
		<balance></balance>
	</div>
	<script src="../lib/vue.js"></script>
	<script type="text/javascript">
		// 父组件,包含了子组件show
		Vue.component('balance', {
			template:` 
			<div>
			    //@show-balance是事件名，被下面$emit调用，执行show_balance()方法
				<show @show-balance="show_balance()"></show> 
				<div v-if="show">余额： ￥ 33</div>
			</div>
			`,
			methods: {
				show_balance: function (){
					this.show = true
				}
			},
			data: function (){
				return {
					//show变量，用于判断是否显示，默认为false，不显示
					show: false
				}
			}
		});
		<!--子组件-->
		Vue.component('show', {
			// 给按钮添加点击监听事件
			template: `
				<button @click="on_click()">显示余额</button>
			`,
			methods: {
				on_click: function(){
					//绑定了show-balance方法
					this.$emit('show-balance') 
				}
			}
		});
		new Vue({
			el: '#app',
		})
	</script>
</body>
</html>
```
---
title: Vue 组件通信 | 练习
date: 2020-02-27 10:02:46
tags: Vue
categories: Vue.js
---
<p align="left">
	这个练习目的是比较使用语法糖和不使用语法糖的效果，发现使用v-model绑定元素之后，通过组件去改变就容易很多，代码量可以少一点
</p>
<!--more-->
<div align="center">
	<img src="/images/vue-practice3.gif" alt="效果演示" align="center">
</div>

---
<h4 align="center">未使用语法糖</h4>
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
		<p>total:{{ total }}</p>
		<!--使用v-on(这里使用了语法糖 @ )添加了两个事件监听器-->
		<!--两个事件会触发handleGetTotal事件-->>
		<my-item @increase="handleGetTotal" @reduce="handleGetTotal"></my-item>
	</div>	
	<script src="lib/vue.js"></script>
	<script>
		Vue.component('my-item', {
			template: `
				<div>
					<button @click="increase()">+1</button>
					<button @click="reduce()">-1</button>
				</div>
			`,
			data: function (){
				return {
					counter: 0
				}
			},
			methods: {
				//点击+1按钮，触发此事件，counter加1，随后将更新的counter通过监听的increase监听器发送出去
				increase: function (){
					this.counter++;
					this.$emit('increase', this.counter);
				},
				//点击-1按钮，触发此事件，counter减1，随后将更新的counter通过监听的increase监听器发送出去
				reduce: function (){
					this.counter--;
					this.$emit('reduce', this.counter);
				}
			}
		});
		var app = new Vue({
			el: '#app',
			data: {
				total: 0
			},
			methods: {
				//这个函数有一个参数，参数是组件按钮通过$emit()发出传过来的数据
				//得到数据后，再把数据绑定到total，这样就是动态的
				handleGetTotal: function (data){
					this.total = data;
				}
			}
		})
	</script>
</body>
</html>
```
<h4 align='center'>使用了语法糖</h4>
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
		<p>总数： <span style="color: red; font-size: 30px;">{{ total }}</span></p>	
		<!-- 使用了v-model绑定了total -->
                <!--可以使用语法糖-->
		<my-item v-model="total"></my-item>
	</div>
	<script src="lib/vue.js"></script>
	<script>
		Vue.component('my-item', {
			template: `
				<div>
					<button @click='increase()'>+1</button>
					<button @click='reduce()'>-1</button>
				</div>
			`,
			data: function (){
				return {
					counter: 0
				}
			},
			methods: {
				increase: function(){
					this.counter++;
					// 由于上面使用了v-model，这里就可以使用语法糖，直接用$emit绑定input，再传递数据
					this.$emit('input', this.counter);
				},
				reduce: function (){
					this.counter--;
					// 由于上面使用了v-model，这里就可以使用语法糖，直接用$emit绑定input，再传递数据
					this.$emit('input', this.counter);
				}
			}
		})
		var app = new Vue({
			el: '#app',
			data: {
				total: 0
			}
		})
	</script>
</body>
</html>
```
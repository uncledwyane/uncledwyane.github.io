---
title: Vue-基础 购物车小组件
date: 2020-02-10 10:44:18
categories: Vue.js
tags: Vue
---

<center>一个基于Vue基础指令的简单的购物车</center>
<!--more-->

**Demo**

<div align='center'>
    <img src='https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/demo_shop.gif'/>
</div>



<center>index.html</center>
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Shop</title>
		<link rel="stylesheet" href="style.css">
	</head>
	<body>
		<div id="app" v-cloak>
			<template v-if="list.length">
				<table>
					<thead>
						<tr>
							<th></th>
							<th>商品名称</th>
							<th>商品单价</th>
							<th>购买数量</th>
							<th>操作</th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(item, index) in list">
							<td>{{ index + 1 }}</td>
							<td>{{ item.name }}</td>
							<td>{{ item.price }}</td>
							<td>
								<button class='fun' type="button" @click="handleReduce(index)":disabled="item.count === 1">-</button>
								{{ item.count }}
								<button class='fun' type="button" @click="handleAdd(index)">+</button>
							</td>
							<td>
								<button @click="handleRemove(index)">移除</button>
							</td>
						</tr>
					</tbody>
				</table>
				<div>总价：￥ {{ totalPrice }}</div>
			</template>
			<div v-else>购物车为空</div>
		</div>
		<script src="../lib/vue.js"></script>
		<script src="index.js"></script>
	</body>
</html>

```
<center>index.js</center>
```javascript
var app = new Vue({
	el: '#app',
	data: {
		list: [
			{
				id: 1,
				name: 'iPhone 7',
				price: 6188,
				count: 1
			},
			{
				id: 2,
				name: 'iPad Pro',
				price: 2888,
				count: 1
			},
			{
				id: 3,
				name: 'MacBook Pro',
				price: 21488,
				count: 1
			}
		],
		text: 'ss'
	},
	computed: {
		totalPrice: function (){
			var total = 0;
			for(var i = 0; i < this.list.length; i++){
				var item = this.list[i];
				total += item.price * item.count;
			}
			return total.toString().replace(/\B(?=(\d{3})+$)/g, ',');
		}
	},
	methods: {
		handleReduce: function (index){
			if(this.list[index].count === 1) return;
			this.list[index].count--;
		},
		handleAdd: function (index){
			this.list[index].count++;
		},
		handleRemove: function (index){
			this.list.splice(index, 1);
		}
	}
});
```
<center>style.css</center>
```css
*{
	margin: 0;
	padding: 0;
	font-family: 'XHei';
}
#app{
	width: 500px;
	height: 500px;
	padding: 20px;
	border-radius: 10px;
	box-shadow: 0 0 30px rgba(0,0,0,.1);
	margin: 20px;
}
table{
	width: 500px;
	height: 200px;
	border: #0000FF solid 1px;
	border-collapse: collapse;
	margin-bottom: 20px;
}
th{
	border: 1px solid #0000FF;
	text-align: center;
}
th:hover{
	background-color: #c4e4ff;
}
tbody{
	vertical-align: middle;
}
td{
	border:solid #3190E8 1px;
	text-align: center;
}
td:hover{
	background-color: #F5F5F5;
}
button.fun{
	width: 20px;
}
```
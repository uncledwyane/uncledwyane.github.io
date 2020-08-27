---
title: Vue filter
date: 2020-02-18 20:26:30
tags: Vue
categories: Vue.js
---
<center>使用过滤器过滤日期</center>
<!--more-->

<div align="center">
	<img src="https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/demo_filter.gif" alt="效果演示">
</div>

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
	<div id="app">
		<!--默认日期，不会刷新-->
		默认日期： {{ default_date }}
		<hr>
		<!--使用过滤器过滤日期-->
		过滤后日期： {{ date | fromDate }}
	</div>
	<script src="lib/vue.js"></script>
	<script type="text/javascript">
		//把小于10的数变成0+这个数，例01，02
		var pddDate = function (value){
			return value < 10 ? '0' + value : value; 
		};
		var app = new Vue({
			el: '#app',
			data: {
				date: new Date(),
				default_date: new Date()
			},
			// 这是一个自定义过滤器，把日期格式化一下
			filters:{
				fromDate: function (){
					var date = new Date();
					var year = pddDate(date.getFullYear());
					var month = pddDate(date.getMonth() + 1);
					var day = pddDate(date.getDay());
					var hours = pddDate(date.getHours());
					var minutes = pddDate(date.getMinutes());
					var seconds = pddDate(date.getSeconds());
					return year + '-' + month + '-' + day + ' ' + hours + ':' + minutes + ':' + seconds; 
				}
			},
			mounted: function(){
				//声明一个变量指向this，防止直接用this导致指向不明
				var _this = this;
				//设置一个定时器，每秒刷新一次
				setInterval(function(){
					_this.date = new Date;
				},1000)
			}
		});
	</script>
</body>
</html>
```
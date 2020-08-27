---
title: Vue 自己的小练习
date: 2020-02-21 20:26:49
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

<center>代码如下</center>
```html
<!DOCTYPE html>
<html lang="zh">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta http-equiv="X-UA-Compatible" content="ie=edge">
	<title></title>
	<style type="text/css">
		.img{
			width: 512px;
			height: 360px;
			border-radius: 5px;
			overflow: hidden;
		}
		.img:hover{
		}
		*{
			margin: 0;
			padding: 0;
			text-align: center;
		}
		.wrap{
		}
		.photo_show{
			display: block;
			width: 700px;
			height: 360px;
			padding: 19px;
			box-shadow: 0 0 30px rgba(0, 0, 0, .1);
			text-align: center;
			border-radius: 20px;
			margin: 150px auto;
		}
		.btn{
			position: relative;
			transform: translateY(-140px);
			transition: .3s;
		}
		.btn:hover{
			cursor: pointer;
		}
	</style>
</head>
<body>
	<div class="wrap">
		<div class="photo_show" id="app">
			<img @click="start_last()" src="../images/last.png" class="btn btn-last">
			<img :src="img_link" class="img img1">
			<img @click="start_next()" src="../images/next.png" class="btn btn-next">
		</div>
	</div>
	<script src="../lib/vue.js"></script>
	<script type="text/javascript">
		var link_index = 1;
		var app = new Vue({
			el: '#app',
			data: {
				img_link: '../images/' + link_index + '.png'
			},
			methods: {
				start_next: function (){
					if(link_index < 6){
						link_index++
						this.img_link = '../images/' + link_index + '.png';
					}else{
						this.img_link = '../images/1.png';
						link_index = 1;
					}
				},
				start_last: function (){
					if(link_index > 1){
						link_index--;
						this.img_link = '../images/' + link_index + '.png';
					}else{
						this.img_link = '../images/6.png';
						link_index = 6;
					}
				}
			},
			mounted: function (){
				setInterval(this.start_next(), 1000);
			}
		})
	</script>
</body>
</html>

```
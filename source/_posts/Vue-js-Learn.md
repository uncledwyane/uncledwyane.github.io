---
title: Vue.js入门
date: 2019-11-05 17:10:31
categories: Vue.js
tags: Vue
---
<center>Vue基础的指令</center>
<!--more-->
**<center>v-for</center>**
```html
<div id="player">
    <ul>
        <li v-for="player in nba">{{ player.name }}</li>
    </ul>
</div>
```
```javascript
var player = new Vue({
	el: '#player',
	data: {
		nba: [
			{ name: 'Dwyane Wade' },
			{ name: 'Lebron  James' },
			{ name: 'Anthony Davis' }
		]
	}
})   
```
**<center>v-on 和 v-if</center>** 
```html
<div id="app">
	<button v-if="showBtn" v-on:click="handleClick">Click me</button>
</div>
```
```javascript
new Vue({
	el: '#app',
	data:{
		showBtn: true
	},
	methods:{   
		handleClick: function(){
			console.log('Clicked!');
		}
	}
})
```
**<center>v-model 双向数据绑定</center>**
```html
<div id="app">
	<input type="text" v-model="name" placeholder="请输入您的名字">
	<h1>你好, {{ name }}</h1>
</div>
<script>
	new Vue({
	el: '#app',
	data: {
		name: ''
	}
	})
</script>

```
<center>vue实现文字滚动显示效果</center>

```html
<div id="app">
			<!--v-on可以缩写例如：v-on:click  |  缩写为: @click  -->
			<input type="button" value="MoveIt" @click="move()">
			<input type="button" value="SlowIt" v-on:click="stop()">
			<input type="button" value="UseThis" v-on:click="usethis()">
			<h3>{{ text }}</h3>
		</div>
		<script>
			var app = new Vue({
				el: '#app',
				data: {
					text: 'I will success!',
					intervalId: null //在data上定义一个定时器的ID,方便methods中访问并改变值
				},
				methods: {
					move(){
						var _this = this
						//防止this指向不明
						if(this.intervalId != null) return;
						this.intervalId = setInterval(function(){
							//在vue实例中,要获取data上的数据,或者想要调用methods里面的方法,需要用到this来调用
							var start = _this.text.substring(0,1); 
							//获取到text的头一个字符
							var end = _this.text.substring(1); 
							//获取到text的最后一个字符
							_this.text = end + start;
							//重新拼接得到新的字符串,赋值给this.text
							
							//注意: 这个app的vue实例会自动监听自己身上data中所有数据的改变,
							//只要数据一发生改变,就会自动把最新的数据从data中同步到页面中去,
							//[好处：只需要关心数据，不需要考虑如何重新渲染DOM页面]
						},100)
					},
					stop(){
						clearInterval(this.intervalId);
						this.intervalId = null;
					},
					usethis: function(){
						alert(this.text); //这里的text是data中的text的值
					}
				}
			})
			
			
		</script>

````
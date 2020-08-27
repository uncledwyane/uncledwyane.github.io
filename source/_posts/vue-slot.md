---
title: Vue slot的使用
date: 2020-02-29 17:03:11
tags: Vue
categories: Vue.js
---
<h4 align="left">个人理解</h4>
<p align="left">slot是一个插槽，和vue使用data的数据{{ data }}一样，也和input的placeholder一样，一个占位符，父组件内没有东西的时候，就会显示slot的默认内容，父组件有内容就会覆盖slot的默认内容</p>
<p align="left">可以在组件模板内给每个slot一个name，当使用组件时，就可以通过slot的name来动态更新组件内容。</p>
<!--more-->
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Document</title>
	<style>
		.panel{
			border: 1px solid #000;
			border-radius: .2em;
		}
		.panel > *{
			padding: 15px;
		}
		.title{
			border-bottom: 1px solid #000;
		}
		.content{
			border-bottom: 1px solid #000;
		}
	</style>
</head>
<body>
	<div id="app">
		<panel>
			<div slot="title">This is title</div>
			<div slot="content">
				Lorem ipsum, dolor sit amet consectetur adipisicing elit. Illum ex blanditiis, delectus ab, placeat deserunt fuga at suscipit numquam pariatur quia perferendis optio ea repudiandae voluptatum, nobis omnis eos tempora?
			</div>
			<div slot="footer">This is footer</div>
		</panel>
	</div>
	<template id="panel">
		<div id="app" class="panel">
			<div class="title">
				<slot name="title">title</slot>
			</div>
			<div class="content">
				<slot name="content">Content</slot>
			</div>
			<div class="footer">
				<slot name="footer">Footer</slot>
			</div>
		</div>
	</template>
	<script src="lib/vue.js"></script>
	<script>
		Vue.component('panel', {
			template: '#panel',

		})
		var app = new Vue({
			el: '#app',

		})
	</script>
</body>
</html>
```
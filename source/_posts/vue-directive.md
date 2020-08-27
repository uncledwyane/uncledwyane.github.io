---
title: Vue | 自定义指令
date: 2020-03-11 10:37:08
tags: Vue
categories: Vue.js
---
<p>效果： 通过自定义指令给渲染的文本添加颜色</p>
<p>自定义指令有两种方式：可以自定义私有`directives`或者公有指令`Vue.directive('name', {bind, inserted, updated})`</p>
<!--more-->
<h4 align="center">代码</h4>
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
        <p v-color="'red'">{{ msg }}</p>
    </div>
    <script src="lib/vue.js"></script>
    <script>
        //定义全局指令
        Vue.directive('color', {
            bind: function (el, binding) { 
                // 每当指令绑定到元素上的时候，会立即执行这个 bind 函数，只执行一次
                // 注意： 在每个 函数中，第一个参数，永远是 el ，表示 被绑定了指令的那个元素，这个 el 参数，是一个原生的JS对象
                // 在元素 刚绑定了指令的时候，还没有 插入到 DOM中去，这时候，调用 focus 方法没有作用
                //  因为，一个元素，只有插入DOM之后，才能获取焦点

                //binding为传过来的参数
                el.style.color = binding.value
            },
            inserted: function (el) {  // inserted 表示元素 插入到DOM中的时候，会执行 inserted 函数【触发1次】
                // 和JS行为有关的操作，最好在 inserted 中去执行，放置 JS行为不生效
            },
            updated: function (el) {  // 当VNode更新的时候，会执行 updated， 可能会触发多次

            }

        })
        var app = new Vue({
            el: '#app',
            data: {
                msg: 'Hello Vue.js'
            },
            //定义私有指令
            // directives: {
            //     'color': function (el){
            //         el.style.color = 'red'
            //     }
            // }
        })
    </script>
</body>
</html>
```
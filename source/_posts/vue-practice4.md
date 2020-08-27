---
title: Vue实战 | 输入框组件
date: 2020-03-04 13:47:44
tags: Vue	
categories:	Vue.js
---
<p align="left"><span style="color: hotpink">笔记：</span>在index-number.js中Vue组件内的watch里，每个成员的方法自带两个参数，前面是新的值，后面是旧的值，如下图所示</p>
<div align="center">
	<img src="/images/vue-note/watch_note1.png" alt="示例">
</div>
<p align="left"><span style="color: hotpink">收获：</span>组件内的数据是用props里定义的变量来传递，相当于一个中间人。</p>
<p>点击下面<span style="color: red">阅读更多</span>查看代码</p>
<!--more-->
<center>index.html</center>
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
        <input-number v-model='value' :max='10' :min='0'></input-number>
    </div>
    <script src="../../../lib/vue.js"></script>
    <script src="index-number.js"></script>
    <script src="index.js"></script>
</body>
</html>
```
<center>index.js</center>

```javascript
new Vue({
    el: '#app',
    data: {
        value: 5
    }
})
```
<center>index-number.js</center>

```javascript
function isValueNumber(value){
    return (/(^-?[0-9]+\.{1}\d+$) | (^-?[1-9][0-9]*$) | (^-?0{1}$)/).test(value + '');
};
Vue.component('input-number', {
    template: `
        <div class='input-number'>
            <input type='text' :value='currentValue' @change='handleChange'/>
            <button @click='handleDown' :disable='currentValue <= min'>-</button>
            <button @click='handleUp' :disable='currentValue >= min'>+</button>
        </div>
    `,
    props: {
        max: {
            type: Number,
            default: Infinity
        },
        min: {
            type: Number,
            default: -Infinity
        },
        value: {
            type: Number,
            default: 0
        }
    },
    data: function (){
        return {
            currentValue: this.value
        }
    },
    watch: {
        currentValue: function (val, oldVal){
            this.$emit('input', val);
            this.$emit('on-change', val);
            this.$emit('old', oldVal);
        },
        value: function(val){
            this.updateValue(val);
        }
    },
    methods: {
        updateValue: function (val){
            if (val > this.max) val = this.max;
            if (val < this.min) val = this.min;
            this.currentValue = val; 
        },
        handleDown: function (){
            if (this.currentValue <= this.min) return;
            this.currentValue -= 1;
        },
        handleUp: function(){
            if (this.currentValue >= this.max) return;
            this.currentValue += 1;
        },
        handleChange: function (event){
            var val = event.target.value.trim();
            var max = this.max;
            var min = this.min;

            if(isValueNumber(val)){
                val = Number(val);
                this.currentValue = val;

                if(val > max){
                    this.currentValue = max;
                }else if(val < min){
                    this.currentValue = min;
                }
            }else{
                event.target.value = this.currentValue;
            }
        }
    },
    mounted: function (){
        this.updateValue(this.value);
        this.$on('old', function (val){
            console.log(val);
        })
    }
})
```
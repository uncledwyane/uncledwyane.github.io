---

title: Vue中监听数据变化的两种方式
date: 2020-03-25 11:52:30
tags: Vue
categories: Vue.js

---
当vue中的数据有变化，例如用户输入新的数据之后，如何处理更新后的数据？这就提供了针对这种情况的两种方式。
<!--more-->
1. 使用keyup
```html
	<div id="app">

    <!-- 分析： -->
    <!-- 1. 我们要监听到 文本框数据的改变，这样才能知道 什么时候去拼接 出一个 fullname -->
    <!-- 2. 如何监听到 文本框的数据改变呢？？？ -->

    <input type="text" v-model="firstname" @keyup="getFullname"> +
    <input type="text" v-model="lastname" @keyup="getFullname"> =
    <input type="text" v-model="fullname">

  </div>
```
```javascript
	<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstname: '',
        lastname: '',
        fullname: ''
      },
      methods: {
        getFullname() {
          this.fullname = this.firstname + '-' + this.lastname
        }
      }
    });
  </script>
```
2. 使用watch, watch中监听添加的需要被监听的组件，用一个带有两个参数的函数处理更新后的数据
```html
<div id="app">

    <input type="text" v-model="firstname"> +
    <input type="text" v-model="lastname"> =
    <input type="text" v-model="fullname">

  </div>
```
```javascript
<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstname: '',
        lastname: '',
        fullname: ''
      },
      methods: {},
      watch: { // 使用这个 属性，可以监视 data 中指定数据的变化，然后触发这个 watch 中对应的 function 处理函数
        'firstname': function (newVal, oldVal) {
          // console.log('监视到了 firstname 的变化')
          // this.fullname = this.firstname + '-' + this.lastname

          // console.log(newVal + ' --- ' + oldVal)

          this.fullname = newVal + '-' + this.lastname
        },
        'lastname': function (newVal) {
          this.fullname = this.firstname + '-' + newVal
        }
      }
    });
  </script>
```
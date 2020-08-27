---
title: Vue 实战|列表管理
date: 2020-03-10 13:45:14
tags: Vue
categories: Vue.js
---
<div align="center">
	<img src="https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/carlist.gif" alt="Demo">
</div>
<p>一个针对之前所学内容做的练习，用BootStrap做的样式，学习了js中几种新的遍历数组的方法（forEach, some, filter, findIndex）</p>
<p>在渲染列表的时候，没有直接使用data中carList的内容，而是对data中的carList用自定义的search方法进行了处理得到的返回值，便于查找</p>
<p>由于初始化时搜索的输入框没有内容，所以可以吧carList中的全部内容渲染到table里，当输入框输入了内容，table里渲染的内容就是在carList中匹配到的数据，这样搜索功能就实现了</p>

---
<!--more-->
<h4 align="center">源码</h4>
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="lib/bootstrap.min.css">
    <script src="lib/vue.js"></script>
    <style>
        [v-cloak]{
            display: none;
        }
        .l1{
            margin-left: 20px;
        }
        button{
            margin-left: 20px;
        }
        .panel-body{
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div id="app" v-cloak>
        <div class="panel panel-primary">
              <div class="panel-heading">
                    <h3 class="panel-title">CarList</h3>
              </div>
              <div class="panel-body form-inline">
                    <label class="l1">
                        <span style="margin-right: 10px;">Id: </span>
                        <input type="text" class="form-control" v-model='id'>
                    </label>
                    <label for="" class="l1">
                        <span style="margin-right: 10px;">Name:</span> 
                        <input type="text" class="form-control" v-model='name'>
                    </label>
                    <button class="btn btn-primary" @click='add'>添加</button>
                    <label for="" class="l1">
                        <span style="margin-right: 10px;">Search:</span>
                        <input type="text" class="form-control" v-model='keywords'>
                    </label>
              </div>
        </div>
        <table class="table table-bordered table-hover">
            <thead>
                <tr>
                    <th>id</th>
                    <th>名称</th>
                    <th>添加时间</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <!--第一种，直接从data里的carList获取数据-->
                <!-- <tr v-for='(item, index) in carList'>
                    <td>{{ index + 1 }}</td>
                    <td>{{ item.name }}</td>
                    <td>{{ item.time | formatDate }}</td>
                    <td>
                        <button class="btn btn-danger" @click.present='remove(index)'>删除</button>
                    </td>
                </tr> -->
                <!--第二种，自定义一个search方法，同时把搜索的
                    关键字通过传参传递给了search方法-->
                <tr v-for='item in search(keywords)'>
                    <td>{{ item.id }}</td>
                    <td>{{ item.name }}</td>
                    <td>{{ item.time | formatDate }}</td>
                    <td>
                        <button class="btn btn-danger" @click='remove(item.id)'>删除</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</body>
</html>
```
```javascript
    <script>
        var DateFormat = function (value){
            return value < 10 ? '0' + value : value;
        };
        var app = new Vue({
            el: '#app',
            data: {
                id: '',
                name: '',
                keywords: '',
                carList: [
                    {
                        id: 1,
                        name: '宝马 M4',
                        time: new Date()
                    },
                    {
                        id: 2,
                        name: '奔驰 C63s',
                        time: new Date()
                    },
                    {
                        id: 3,
                        name: '奥迪 RS6 Avent',
                        time: new Date()
                    }
                ]
            },
            filters: {
                formatDate: function (){
                    var date = new Date();
                    var year = date.getFullYear();
                    var month = DateFormat(date.getMonth() + 1);
                    var day = DateFormat(date.getDay());
                    var hours = DateFormat(date.getHours());
                    var minutes = DateFormat(date.getMinutes());
                    var seconds = DateFormat(date.getSeconds());

                    //ES6语法，使用占位符可以简化语句
                    return `${ year }-${ month }-${ day } ${ hours }:${ minutes }:${ seconds }`;
                }
            },
            methods: {
                remove: function(index){
                    this.carList.splice(index, 1);
                },
                add: function (){
                    var car = {
                        id: this.id,
                        name: this.name,
                        time: new Date()
                    };
                    if(this.id == '' || this.name == ''){
                        alert('请输入内容！');
                    }else{
                        this.carList.push(car);
                        this.id = '';
                        this.name = '';
                    }
                },
                search: function (keywords){
                    //注意： forEach, some, filter, findIndex 这些都是属于数组的新方法，
                    //都会对数组中的每一项进项遍历，执行相关操作
                    // 第一种遍历方法
                    // var newList = [];
                    // this.carList.forEach(item => {
                    //     if(item.name.indexOf(keywords) != -1){
                    //         newList.push(item);
                    //     }
                    // });
                    // return newList;
                    // 第二种遍历方法
                    return this.carList.filter(item => {
                        if(item.name.includes(keywords)){
                            return item;
                        }
                    })
                    
                }
            }
        });
    </script>
```
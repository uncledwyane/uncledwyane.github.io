---
title: Vue路由笔记
date: 2020-03-22 11:45:53
tags: Vue
categories: Vue.js
---
<center></center>
<!--more-->

**redirect重定位，给路由指定默认的组件，打开就是指向的默认组件**

```javascript
var router = new VueRouter({
    route: [
        //这里path后不是conponent，而是用redirect指向一个组件路径
        {path: '/', redirect: '/login'},
        {path: '/login', conponent: login}
    ]
})
```

**给激活的router-link添加高亮的两种方式**

- 直接给默认的linkActiveClass添加样式
- 改变router-link的默认类，使用构造方法VueRouter中的linkActiveClass来给自定义新的类

```javascript
var router = new VueRouter({
    route: [
        {path: '/login', conponent: login}
    ],
    linkActiveClass: 'myActive', //改变了默认的类，可以给这个类指定样式使得激活后高亮
})
```

**给router-link添加参数传递的两种方式**

1. 在router-link中的to中加入参数：?id=10&name=username，组件中可以通过**$route.query.id**和**$route.query.name**来获取传递的参数值

```html
<router-link to='/login?id=10&name=username'></router-link>
```

2. 在路由的构造函数的routes的path中使用**/:name**来说明这里是会有参数传递的，也可以理解为占一个位置，可以通过**$route.params.id**和**$route.params.name**获取

```javascript
var router = new VueRouter({
    routes: [
        {path: '/login/:id/:name', conponent: login}
    ]
})
```

​	传递参数： 这里的实际参数id就为10，name就是username

```html
<router-link to='/login/10/username'></router-link>
```

**路由嵌套**

​	在路由构造函数实例中的**path**中添加**children**指向子路由，组件中还有router-link指向子组件



```javascript
var parent = {
    template: `
		<div>
			Parent
			<router-link to='/parent/son'></router-link>
		</div>
	`
}
var router = new VueRouter({
    route: [
        {
            path: '/parent',
            component: parent,
            children: [
                {path: 'son', component: son}
            ]
        }
    ]
})
```

**使用命名视图给路由组件添加名称，方便给每个组件渲染样式**

```html
<div id='app'>
    <router-view></router-view>
    <router-view name='left'></router-view>
    <router-view name='main'></router-view>
</div>
```

```javascript

var header = {
    template: '<h1>顶部</h1>'
}
var leftBox = {
    template: '<h1>左侧侧边栏</h1>'
}
var rightBox = {
    template: '<h1>主体区域</h1>'
}
var router = new VueRouter({
    routes: [
        { 
            path: '/', 
            components: {
                'default': header,
                'left': leftBox,
                'main': rightBox
            }
        }
    ]
})
var app = new Vue({
    el: '#app',
    router: router,
})

```


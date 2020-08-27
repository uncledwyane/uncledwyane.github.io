---

title: Vue路由嵌套
date: 2020-03-25 09:46:43
tags: Vue
categories: Vue.js

---

**必须注意的两个点**

1. 子路由的**router-link**中的path必须是完整的path（带父级path），例如父级是/parent，子级是/son，那么子路由的router-link中的path必须为/parent/son；
2. routes里的children中的子路由的path前面不能带 <span style='color: red'>/</span>，例如：

```javascript
routes: [
    {
        path: '/parent',
        component: parent,
        children: [
            {
                path: 'son',//这里就没有前面的 /
                component: son
            }
        ]
    }
]
```

点击<span style='color: red'>阅读更多</span>查看代码

<!--more-->
<div align='center'>
    <img src="/images/vue_router/vue_router1.gif" alt="" style="width: 400px;">
    <img src="/images/vue_router/vue_router1-1.gif" alt="" style="width: 400px;">
</div>

```html
<div id='app'>
        <router-link to="/home">首页</router-link>
        <router-view></router-view>
    </div>
    <template id="home">
        <div>
            <h3>Home</h3>
            <router-link to="/home/son">Son</router-link>
            <router-view></router-view>
        </div>
    </template>
    <template id="son">
        <div>
            <h4>I am Son</h4>
            <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Nesciunt in voluptatibus expedita repudiandae. Soluta distinctio quod quos dolorem blanditiis magni molestiae, minima dolores ipsum! Minima possimus sunt eius quis debitis!</p>
        </div>
    </template>
```

```javasc
var home = {
            template: '#home'
        }
        var son = {
            template: '#son'
        }
        var router = new VueRouter({
            routes: [
                {
                    path: '/home',
                    component: home,
                    children: [
                        {path: 'son', component: son}
                    ]
                },
            ],
            linkActiveClass: 'myActive'
        })
        var app = new Vue({
            el: '#app',
            router: router,
        })
```


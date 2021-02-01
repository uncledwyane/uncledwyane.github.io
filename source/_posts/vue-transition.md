---
title: vue 过渡
date: 2021-02-01 14:55:59
tags: 过渡
categories: Vue.js
---
在vue项目中的过渡效果，碰到的问题及解决过程。
<!--more-->
<p style='text-indent: 2em;'>
    最近在自己练手的项目中使用了transition给组件切换时增加过渡效果，结合官方文档的示例添加的效果始终让我不满意，想要的效果是下面官方这种：
</p>
<img src='https://raw.githubusercontent.com/uncledwyane/imageBed/master/img/bandicam.gif'/>

我实现的是这种，虽然进入和离开都有了，但是和官方这个按钮动画比还是比较生硬，进入和离开不自然：

<img src="https://raw.githubusercontent.com/uncledwyane/imageBed/master/img/bandicam%202021-02-01.gif"/>

仔细再看了一下文档，看到了这句话：

<img src="https://raw.githubusercontent.com/uncledwyane/imageBed/master/img/20210201154911.png"/>

马上在router-view涉及到的组件根div上添加绝对定位，效果出来了！！！果然要仔细阅读文档啊！

<img src="https://raw.githubusercontent.com/uncledwyane/imageBed/master/img/bandicam%202021-02-011.gif"/>



#### 总结：
1. 在transition标签中设置过渡模式 mode
```html
<transition name='my_transition' mode='in-out'>
    <router-view></router-view>
</transition>
```

2. 设置过渡动画效果
```css
.my_transition-enter, .my_transition-leave-to{
    transform: translateY(-100%);
    opacity: 0;
}
.my_transition-enter-active,.my_transition-leave-active{
    transition: all .5s ease;
}
.my_transition-enter-to{
    opacity: 1;
}
```
3. 给router-view涉及到的组件根元素添加绝对定位
```css
.root_div{
    position: absolute;
}
```

完成！！！





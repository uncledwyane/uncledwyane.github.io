---
title: webpack中跨域问题(已解决)
date: 2020-04-07 16:36:03
tags: 跨域
categories: webpack
---
<img src="https://s1.ax1x.com/2020/04/07/GgUB8J.jpg" border="0" />

<p>
<span style='letter-spacing: 2px;'>
	最近几天学完了vue的基本课程，想跟着教程做个实战，由于教程不是最新的，里面涉及到的接口就失效了，于是网上找了个接口，使用axios请求数据发现控制台居然报了上面这个错，还是小白的我一脸懵b，马上百度，发现一个专有名词： <span style='color: red;font-weight: bolder;font-size: 16px;'>跨域</span>，跨域的解释这里就不啰嗦了，百度一大堆。为了解决这个问题，翻了很多篇教程，无果.........
</span>
</p>
<!--more-->

<p style='letter-spacing: 2px;'>
一直解决不了这个问题，作为小白真想放弃，但第二天还是决定认真看一看其他教程，最终找到了webpack中解决跨域问题的办法，就是在`webpack.config.js`中加入以下语句：
</p>
```javascript
module.exports = {
	devServer: {
    	proxy: {
        	'/api': {
            	target: 'http: www.exsample.cn',
                changeOrigin: true
            }
        }
    }
}
```
重点就是这个`changeOrigin: true`，
在上面的代码中,`/api`就是在请求中，遇到这个开头的就马上代理为本地服务器，比如要请求的网络地址是`http://jiekou.cn/api/data.json`，那么经过webpack这段配置文件处理过后请求的地址就转变为本地服务器地址`http://localhost:8080/api/data.json`，这样本地服务器去请求接口数据的头部都是`localhost:8080`了


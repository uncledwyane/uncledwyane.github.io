---
title: 关于CSS3的一些属性
date: 2019-11-17
tags: CSS
categories: web学习
---

<!--more-->

**1.auto,vw,vh**
```css
width:auto; //子元素会撑开至父元素的宽度，但会减去自身Margin和Padding的大小，不会溢出。
width：100%; //子元素会撑开父元素至的宽度，但如果自身还有Margin或者Padding，则宽度是父元素的宽度加上Margin和Padding的宽度，会溢出。
width:50vw; //这里的vw表示视窗宽度的百分比，1vw就是50%的宽度。
height:50vh; //这里的vh视窗高度的百分比，50vh就是视窗高度的50%。
```
**2.css3的新属性：vw、vh、vmin、vmax**
```css
vw：视窗宽度的百分比（1vw 代表视窗的宽度为 1%）
vh：视窗高度的百分比
vmin：当前 vw 和 vh 中较小的一个值
vmax：当前 vw 和 vh 中较大的一个值
```
**3.vw、vh 与 % 百分比的区别**

 - % 是相对于父元素的大小设定的比率，vw、vh 是视窗大小决定的。
 - vw、vh 优势在于能够直接获取高度，而用 % 在没有设置 body 高度的情况下，是无法正确获得可视区域的高度的，所以这是挺不错的优势。
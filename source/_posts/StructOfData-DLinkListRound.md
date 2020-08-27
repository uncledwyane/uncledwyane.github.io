---
title: 数据结构 循环链表
date: 2020-05-30 18:33:35
tags: 数据结构
categories: 数据结构
---

<div>
    首尾相连，一般的，链表最后一个结点的指针域是空指针，如果将其指向头结点，则使得头尾相连构成循环链表。
</div>

<!--more-->

![](https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/循环链表.jpg)

![](https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/循环链表2.jpg)

<!--more-->

```c
#include<stdio.h>
#include<stdlib.h>
typedef int elemtype;
typedef struct node {
	elemtype data;
	struct node *next;
}LinkList;

 /*合并两个单链表为一个循环链表*/

LinkList *Connect(LinkList *ra, LinkList *rb) {
	LinkList *p;
	p = ra->next; // 保存ra的头结点
	ra->next = rb->next->next; // 将rb 链接在ra 之后
	free(rb->next); // 释放 rb 的头结点
	rb->next = p; // 修改rb 指针
	return rb;
}

// 其余操作和单链表一致
```


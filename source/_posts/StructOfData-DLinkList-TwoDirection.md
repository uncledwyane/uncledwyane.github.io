---
title: 数据结构 双向循环链表
date: 2020-05-30 18:34:00
tags: 数据结构
categories: 数据结构
---

<div>
    双向链表，两个指针，一个指向直接前驱，一个指向直接后继。
</div>



<!--more-->

![](https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/双向循环链表2.jpg)

![](https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/双向循环链表1.jpg)

```c
#include<stdio.h>

/*定义*/
typedef int elemtype; // 结点的数据类型，设为整形
typedef struct dnode { // 定义双向链表结点类型
	elemtype data; // 结点的数据域
	struct node *next, *prior; // 结点的指针域，前驱为prior，后继为next
}DLinkList;// 双向链表类型名为DLinkList

/*带头节点的双向链表的前插运算*/
void DinsertBefore(DLinkList *p, DLinkList *s) {
	s->prior = p->prior; // 修改新结点的前驱指针
	s->next = p; // 修改新结点的后继指针
	p->prior->next = s; // 修改p的前驱节点的后继指针
	p->prior = s;// 修改p的前驱指针
}

/*带头结点的双向链表的删除操作*/
void DDeleteNode(DLinkList *p) {
	p->prior->next = p->next;// 修改结点p的前驱结点的后继指针
	p->next->prior = p->prior;// 修改结点p的后继结点的前驱指针
	free(p); // 释放p的内存空间
}
```


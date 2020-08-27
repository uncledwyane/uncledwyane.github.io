---
title: 数据结构 单链表的基本操作
date: 2020-05-30 17:59:10
tags: 数据结构
categories: 数据结构
---

<div>
    <p>
        链表是通过一组地址任意的存储单元来存储线性表中的数据元素，可以连续也可以不连续。所以在链表中，逻辑上相邻的数据元素在物理上并不一定相邻。
    </p>
</div>



<!--more-->

![](https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/单链表.jfif)

```c
#include<stdio.h>
#include<stdlib.h>

typedef int elemtype;
typedef struct node{
	elemtype data;
	struct node *next;
}LinkList;


/*头插法建立单链表*/

LinkList *Create_LinkListF() {
	elemtype ix;
	LinkList *head, *p;
	head = (LinkList *)malloc(sizeof(LinkList));	// 生成头结点指针 head
	if (head == NULL) {
		return head; // 如果申请失败，返回头指针
	}
	head->next = NULL; 
	printf("请输入数据直到输入0结束:\n");
	scanf("%d", &ix); // 输入第一个数据
	while (ix != 0) { // 输入数据，以 0 结束
		p = (LinkList *)malloc(sizeof(LinkList)); // 生成新结点
		if (p == NULL) { 
			return head; // 申请失败，返回
		}
		p->data = ix; // 将输入的数据赋值给新结点
		p->next = head->next; // 修改新结点的指针域
		head->next = p;		// 修改头结点的指针域
		scanf("%d", &ix); // 读取下一个数据
	}
	return head;  // 返回头结点指针
}

/*尾插法建立单链表*/
LinkList *Create_LinkListR() {
	elemtype ix;
	LinkList *head, *p, *tail; // *head,*tail分别为头指针和尾指针
	head = (LinkList *)malloc(sizeof(LinkList)); // 生成头结点
	if (head == NULL) {
		return head;
	}
	head->next = NULL; // 置头结点指针域为空
	tail = head; //  尾指针指向头结点
	printf("请输入数据直到输入0结束:\n");
	scanf("%d", &ix); // 输入第一个数据
	while (ix != 0) { // 输入数据，以 0 结束
		p = (LinkList *)malloc(sizeof(LinkList)); // 生成新结点
		if (p == NULL) {
			return head; // 申请失败，返回
		}
		p->data = ix; // 将输入的数据赋值给新结点
		tail->next = p; // 修改尾结点指针域指向p
		tail = p; // 修改尾指针
		tail->next = NULL;// 置尾结点指针域为空
		scanf("%d", &ix);// 读取下一个数据
	}
	return head; // 返回头结点指针

}

/*遍历*/
int Print_LinkList(LinkList *head) {
	LinkList *p = head->next;
	if (p == NULL) { 
		// 链表为空，返回值为 0
		return 0;
	}
	while (p != NULL) { // 当前结点不为空
		printf("\t%d", p->data); // 输出当前节点的数据
		p = p->next; // 移动指针指向下一个结点
	}
	return 1;
}

/*求单链表长度*/
int LinkList_Length(LinkList *head) {
	LinkList *p = head; // p指向头结点
	int j = 0;
	while (p->next != NULL){
		p = p->next; // p指向下一个结点
		j++;
	}
	return j;// 遍历完毕，返回长度
}

/*按序号查找*/
LinkList *GetData_LinkList(LinkList *head, int i) {
	LinkList *p;
	int j = 0;
	if (i <= 0) {
		return NULL; // 指定的位置非法，返回NULL
	}
	p = head;
	while (p->next != NULL && j < 1) { 
		p = p->next; // 不是目标节点且还有后继结点时继续查找
		j++;
	}
	if (i == j) return p; // 找到目标结点，返回当前指针
	else return NULL; // 未找到目标结点，返回NULL
}

/*按值查找*/
LinkList *Search_LinkList(LinkList *head, elemtype key) {
	LinkList *p;  // 创建LinkList类型指针变量p
	p = head->next; // p指向头结点
	while (p != NULL) { // p的结点不为空则执行
		if (p->data != key) { // 与 key 值不匹配
			p = p->next; // 移动指针，指向下一个结点
		}
		else {
			break; // 找到，退出循环
		}
	}
	return p; // 返回找到的结点指针或者未找到时的NULL
}

/*插入, 后插*/
void InsertAter_LinkList(LinkList *p, LinkList *s) { // *p指向单链表某一结点，*s指向待插入值的新结点
	s->next = p->next; // 新结点连入链表
	p->next = s; // 修改前趋结点的指针域
}

/*插入，前插*/
void InsertBefore_LinkList(LinkList *head, LinkList *p, LinkList *s) {
	LinkList *q;
	q = head;
	while (q->next != p) { // 从头结点开始搜索结点p的前趋结点
		q = q->next;
	}
	s->next = p; // 修改相应结点的指针域
	q->next = s; 
}

/*在指定序号前插入*/
int InsertNo_LinkList(LinkList *head, LinkList *s, int i) {
	LinkList *p;
	if (i <= 0) {
		p = NULL;
	}
	else if(i == 1){
		p == head;
	}
	else {
		p = GetData_LinkList(head, i - 1); // 搜索第i - 1个结点
	}
	if (p == NULL) return 0; // 不存在第 i 个位置的结点
	else {
		InsertAter_LinkList(p, s); // 调用后插函数
		return 1;
	}
}

/*删除后继结点*/
int DeleteAfter_LinkList(LinkList *p) {
	LinkList *r;
	if (!p) return 0;
	r = p->next;
	if (!r) return 0;
	p->next = r->next; // 修改p的指针，指向r的后继结点，跳过r从而将r从链表上删除
	free(r); // 释放r占用的内存空间
	return 1; // 删除成功返回1
}

/*删除指定结点本身*/
int DeleteNode_LinkList(LinkList *head, LinkList *p) {
	LinkList *r;
	if (p->next != NULL) {
		p->data = p->next->data; // 将后继结点的数据写入到当前节点的数据域
		return (DeleteAfter_LinkList(p));// 删除p的后继结点
	}
	else {
		r = head;
		while (r->next != p) { // 搜索p的前驱结点
			r->next;
			return (DeleteAfter_LinkList(r)); // 删除p的后继结点
		}
	}
}

/*删除指定位置的结点*/
int DeleteNo_LinkList(LinkList *head, int i) {
	LinkList *p, *r;
	if (i <= 0) p = NULL;
	else if (i == 1) p = head;
	else p = GetData_LinkList(head, i - 1); // 搜索第 i-1 个结点
	if (p == NULL) return 0; // 不存在第 i  个结点，删除失败
	else {
		r = p->next;
		if (r == NULL) return 0; // 结点不存在，删除失败
		p->next = r->next; // 删除指定结点
		free(r);
		return 1;
	}
}
/*置空表*/
LinkList *SetNull_LinkList(LinkList *head) {
	while (head->next) {
		DeleteAfter_LinkList(head);
	}
	return head;
}

```


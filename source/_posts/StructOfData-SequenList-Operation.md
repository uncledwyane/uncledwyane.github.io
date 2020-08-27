---
title: 数据结构 线性表的初始化及操作
date: 2020-05-30 15:25:03
tags: 数据结构
categories: 数据结构
---

<div class='notification is-info'>
    <p>
        线性表的基本操作
    </p>
</div>

| 优点                                               | 缺点                       |
| -------------------------------------------------- | -------------------------- |
| 结构简单，易于理解                                 | 存储空间不易扩充           |
| 方便随机访问表中的每个元素                         | 容易造成存储空间的利用率地 |
| 不需要再为表示结点间的逻辑关系而增加额外的存储空间 | 插入删除运算不方便         |



<!--more-->

![](https://cdn.jsdelivr.net/gh/uncledwyane/imageBed/img/线性表.jfif)

```c
#include<stdio.h>
#include<stdlib.h>
#define MAXSIZE 1024
typedef int elemtype;
typedef struct sequlist{
	elemtype data[MAXSIZE];
	int last;
}SequenList;
/*顺序表的初始化*/
SequenList *Init_SequenList() {
	SequenList *L; /*定义顺序表指针变量*/

	/*申请分配内存空间*/
	L = (SequenList *)malloc(sizeof(SequenList)); 
	/*申请分配内存空间成功*/
	if (L != NULL) { 
		 /*设置顺序表的长度last为 -1 表示顺序表为空*/
		L->last = -1;
	}
	return L; // 返回顺序表的首地址
}

/*求顺序表的长度*/
int SequenList_Length(SequenList *L) {
	return(L->last + 1);
}

/*顺序表指定位置插入元素*/
int Insert_SequenList(SequenList *L, elemtype x, int i) {
	/*在顺序表中指定的位置插入值为x的节点*/
	/*L是SequenList类型的指针变量*/
	/*i是指定要插入的位置*/
	int j;
	if (L->last >= MAXSIZE - 1) {   // 判满
		return 0;
	}
	if (i < 1 || i > L->last + 2) {  // 判断插入位置是否合法
		return 0;
	}
	for (j = L->last; j >= i - 1; j--) {  // 在第 i 个位置插入新元素
		L->data[j+1] = L->data[j];    // 结点依次向后移动一个位置
		L->data[i - 1] = x;			// 将 x 插入到第 i 个位置
		L->last = L->last + 1;		// 表长加一
	}
	return 1; // 插入成功，返回 1

}

/*顺序表删除指定位置元素*/
int Delete_SequenList(SequenList *L, int i) {
	int j;
	if (i < 1 || i > L->last + 2) {  // 判断插入位置是否合法
		return 0;
	}
	else {
		for (j = i; j <= L->last; j++) { // 在第 i 个位置删除元素
			L->data[j - 1] = L->data[j]; // 结点依次向前移动一个位置
		}
		L->last - 1; // 表长减 1
	}
	return 1;// 删除成功，返回 1
}

/*取数据元素*/
elemtype GetData_SequenList(SequenList *L, int i) {
	if (i < 1 || i > L->last + 1) { // 判断位置是否合法
		return 0;
	}
	else {
		return (L->data[i - 1]);  // 返回所需结点的值
	}
}

/*查找*/
int Search_SequenList(SequenList *L, elemtype key) {
	int i;
	for (i = 0; i <= L->last; i++) {  // 遍历表，将遍历项依次和 key 进行比较
		if (L->data[i] == key) { // 找到与 key 相等的元素
			return (i + 1);   // 返回其位置
		}
	}
	return 0;
}

/*遍历*/
int Print_SequenList(SequenList *L) {
	int i;
	if (L->last == -1) {
		return 0;  // 空表，返回 0
	}
	for (i = 0; i <= L->last; i++) {
		printf("a[%2d]=%4d\t", i+1, L->data[i]); // 输出表元素
		if ((i + 1) % 5 == 0) printf("\n");  // 控制每行输出的个数
	}
	return 1;
}

void main() {
	SequenList *L;
	L = Init_SequenList();
	if (L == NULL) {
		printf("申请顺序表内存空间失败！程序结束！\n");
		return;
	}
	else {
		printf("申请顺序表内存空间成功！\n");
	}
}
```


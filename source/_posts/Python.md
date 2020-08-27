---
title: Python作业
date: 2019-10-10
tags: Python
categories: 编程
---
<center>Python作业，熟悉Python语法基础</center>

<!--more-->

>题目背景介绍：你准备去海南旅游，现在要订购机票。机票的价格受旺季、淡季影响，而且头等舱和经济舱的价格也不同。假设机票原价是5000元，4~10月为旺季，旺季头等舱打9折，经济舱6折；其他月份为淡季，淡季头等舱5折，经济舱4折，编写程序，根据出行的月份和选择的舱位输出实际机票的价格。

```python
final_price = 0
price = 5000
print('请输入你想出行的月份(1~12)：')
month = eval(input())
print('请输入你想购买的舱位，头等舱输入1，二等舱输入2')
level = eval(input())
if month >= 4 and month <= 10:
    if level == 1:
        final_price = price * 0.9
        print('您的机票价格为：' + str(final_price))
    elif level == 2:
        final_price = price * 0.6
        print('您的机票价格为：' + str(final_price))
else:
    if level == 1:
        final_price = price * 0.5
        print('您的机票价格为：' + str(final_price))
    elif level == 2:
        final_price = price * 0.4
        print('您的机票价格为：' + str(final_price))
```

>题目：输入一批数字，输出其中的最大值和最小值，输入数字0结束.
```python
a = 0
list = []
while a == 0:
    print('请输入一个数:')
    number = eval(input())
    list.append(number)
    if number == 0:
        a != 0
        break
    else:
        continue
list.sort(reverse=True)
print('最大值为:' + str(list[0]))
list.sort()
print('最小值为：' + str(list[1]))
```
>题目：创建一个列表，将员工月薪数据保存到其中，并对列表进行如下操作：
1.添加一名月薪6000的员工至列表末尾
2.插入一名月薪7500的员工到列表中索引为2的位置 
3.移除列表中最后一个数据，并显示移除的值 
4.将列表中的第二个数据的值增加100
5.删除列表中第5个数据 
6.按顺序遍历输出员工的月薪
7.将所有月薪小于5000的员工月薪，修改为5000，并输出其索引值
```python
month_salary = [{'id':'a1','name':'王保华','salary':10000},
                {'id':'a2','name':'李维新','salary':5200},
                {'id':'a3','name':'张强','salary':4700},
                {'id':'a4','name':'张明','salary':3860},
                {'id':'a5','name':'陈鑫','salary':1200},
                {'id':'a6','name':'李牧','salary':8500}]

month_salary.append({'salary':6000})
month_salary.insert(2,{'salary':7500})
month_salary[1]['salary'] += 100
month_salary.pop(4)
for i in range (len(month_salary)):
    if month_salary[i]['salary'] < 5000:
        month_salary[i]['salary'] = 5000
        index = i
        print('月薪低于5000的索引为：' + str(index))
    else:
        month_salary[i]['salary'] = month_salary[i]['salary']
for i in range (len(month_salary)):
    print(month_salary[i])
```
>创建一个字典，将员工工号、姓名、月薪数据保存到字典中，并按要求做如下操作:
1.打印所有员工信息
2.从字典中获取员工工号为“a4”的员工信息，并打印
3.判断是否有工号为“a9”的员工，如果存在，输出该员工信息；否则输出“员工不存在”
4.遍历字典中所有员工信息，并输出
5.添加一名员工数据：工号a7，姓名李梅，月薪9000
6.将工号为a4的员工的月薪修改为4900
7.删除列表中工号为a4的员工数据 
```python
month_salary = {'a1':['王保华',10000],
          'a2':['李维新',10000],
          'a3':['张强',10000],
          'a4':['张明',10000],
          'a5':['陈鑫',10000],
          'a6':['李牧',10000]}
i = 'a9'
print(month_salary)
print(month_salary['a4'])
if i in month_salary:
    employee = month_salary[i]
    print('工号为%s的员工信息：'%i)
    print(employee)
else:
    print('员工不存在')
for number in month_salary:
    print(month_salary['%s'%number])
month_salary['a7'] = ['李梅',9000]
month_salary['a4'][1] = 4900
print(month_salary['a4'][1])
print(month_salary)
del month_salary['a4']
print(month_salary)
```
>统计诗经《桃夭》中出现的汉字和标点的次数。
1.使用字符串保存《桃夭》
2.遍历诗歌中所有的汉字和标点 
3.遍历过程中统计用到了哪些汉字和标点 
4.统计汉字和标点个数使用字典结构 
5.判断字符是否在字典中，如果在，则将该键对应的值加1，如果不在则新创建该键，并赋值1
6.使用for循环遍历输出汉字、标点的使用个数 
```python
poem = """桃之夭夭，灼灼其华。之子于归，宜其室家。桃之夭夭，有蕡其实。之子于归，宜其家室。桃之夭夭，其叶蓁蓁。之子于归，宜其家人。"""
character_counts = {}
for character in poem:
    if character in character_counts:
        character_counts[character] += 1
    else:
        character_counts[character] = 1
for key in character_counts:
    print("%s出现了 %d次"%(key,character_counts[key]))
```
>定义函数接收年份和月份，返回对应的月份有多少天：闰年二月为29天，否则为28天（闰年就是二月有29天的年份，能被4整除但不能被100整除的是闰年，能被400整除的也是闰年）。4,6,9,11月为30天,其余月为31天
```python
def getYear():
    print('请输入一个年份：')
    year = eval(input())
    print('请输入月份：')
    month = eval(input())
    if(year % 4 == 0 and year % 100 != 0 or year % 400 == 0):
        if month == 2:
            print('%d年%d月有29天'%(year,month))
        else:
            print('%d年%d月有29天' % (year, month))
    else:
        if month == 4 or month == 6 or month == 9 or month == 11:
            print('%d年%d月有30天'%(year,month))
        elif month == 2:
            print('%d年%d月有28天' % (year, month))
        else:
            print('%d年%d月有31天' % (year, month))
getYear()
```
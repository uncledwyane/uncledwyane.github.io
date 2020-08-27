---
title: 第一个爬虫项目
date: 2019-11-28 17:11:14
tags: Python,爬虫
categories: Python
---

<center>第一个爬虫项目：爬取前程无忧重庆地区工作</center>
<!--more-->
<center>LoginUI.py</center>

```python
from tkinter import *
import tkinter as tk
import tkinter.messagebox
from SearchItemUI import *
win = Tk()
win.geometry('368x600')
win.title('Login UI')

bg_photo = tk.PhotoImage(file='img/login_bg.png')
bg_label = tk.Label(
    win,
    image=bg_photo,
    compound=tk.CENTER
).pack()
labelimg = PhotoImage(file='img/smile.png')
img_label = Label(win, image=labelimg).place(x=120, y=60)
text_username = Label(win, bg='#68ABFF', fg='#fff', text='账号:', font=('Xhei', '16')).place(x=60, y=240)
text_password = Label(win, bg='#94C3FF', fg='#fff', text='密码:', font=('Xhei', '16')).place(x=60, y=280)
username = StringVar()
password = StringVar()
input_username = Entry(win, textvariable=username, font=('Xhei', '16'), width=14).place(x=120, y=240)
inout_password = Entry(win, textvariable=password, font=('Xhei', '16'), show='*', width=14).place(x=120, y=280)


def check_login():
    username_get = username.get()
    password_get = password.get()
    if username_get == 'user' and password_get == '123456':
        win.destroy()
        start_search()
    elif username_get == '' or password_get == '':
        tk.messagebox.showwarning(title='警告', message='请输入用户名或者密码!')
    else:
        tk.messagebox.showwarning(title='警告', message='用户名或者密码错误!')


btn_login = Button(win, command=check_login, text='登录', font=('Xhei', '16'), bg="#68ABFF", fg="#FFF").place(x=140, y=340)
win.mainloop()
```
<center>SearchItemUI.py</center>

```python
from tkinter import *
import requests
import webbrowser
import re
import tkinter.ttk as ttk
from bs4 import BeautifulSoup


def start_search():
    win = Tk()
    win.title("51Job招聘")
    win.geometry('690x760')
    job_categorie = StringVar()
    keyword = StringVar()
    page_num = IntVar()
    v = IntVar()
    com_type_value = StringVar()

    top = Toplevel(win)
    top.title("详细信息")
    top.geometry('930x724')

    title = ['l1', 'l2', 'l3', 'l4']
    tree = ttk.Treeview(top, column=title, show='headings', height=20, selectmode='browse')
    tree.column('l1', anchor='center', width=300)
    tree.column('l2', anchor='center', width=300)
    tree.column('l3', anchor='center', width=145)
    tree.column('l4', anchor='center', width=145)
    tree.heading('l1', text='职位')
    tree.heading('l2', text='公司名称')
    tree.heading('l3', text='工作地点')
    tree.heading('l4', text='薪资待遇')
    tree.place(x=0, y=0, width=903, height=725)
    vbar = ttk.Scrollbar(top, orient="vertical", command=tree.yview)
    tree.configure(yscrollcommand=vbar.set)
    vbar.place(x=904, y=5, height=720)

    def search_job():
        input_keyword = keyword.get()
        input_page_num = page_num.get()
        select_com_type_value = com_type_value.get()
        the_url = "https://search.51job.com/list/060000,000000,0000,00,9,99,%s,2,%d.html" % (input_keyword,
                                                                                             input_page_num)
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) '
                          'Chrome/78.0.3904.108 Safari/537.36'}
        response = requests.get(the_url, headers=headers)
        response.encoding = 'gbk'
        html = response.text
        soup = BeautifulSoup(html, features='lxml')
        all_div_e1 = soup.select('div[class="el"]')
        span_t1 = soup.find_all('span', {'class': 't1'})
        all_div_jobpositions = soup.find_all('p', {'class': 't1'})
        all_companys = soup.find_all('span', {'class': 't2'})
        all_address = soup.find_all('span', {'class': 't3'})
        all_salary = soup.find_all('span', {'class': 't4'})
        job_list = []
        companys_list = []
        address_list = []
        salary_list = []
        for item in all_div_jobpositions:
            all_titles = item.find_all('a')
            job_list.append(all_titles[0].get_text().strip('\r').strip('\n').strip(' ').strip('.'))
        for get_item in all_companys:
            get_a = get_item.find_all('a')
            for i in get_a:
                com_name = i.get_text().strip('.')
                companys_list.append(com_name)
        for add in all_address:
            the_list = []
            add_value = add.get_text()
            the_list.append(add_value)
            address_list = address_list + the_list
        for salary in all_salary:
            the_list = []
            salary_value = salary.get_text()
            if salary_value == "":
                salary_value = "未知或面谈"
            the_list.append(salary_value)
            salary_list = salary_list + the_list
        address_list.pop(0)
        salary_list.pop(0)
        zipped = zip(job_list, companys_list, address_list, salary_list)
        for zip_item in zipped:
            tree.insert("", 1, value=zip_item)

    def clear_data():
        items = tree.get_children()
        for each_item in items:
            tree.delete(each_item)

    wrap_framl = LabelFrame(win, text="搜索", font=('Xhei', '14'), width=650, height=80) \
        .place(x=20, y=30)
    label_notice = Label(wrap_framl, text='职位关键词:', font=('Xhei', '13')) \
        .place(x=35, y=60)
    keyword_entry = Entry(wrap_framl, textvariable=keyword, font=('Xhei', '13')) \
        .place(x=150, y=60)
    label_page_num = Label(wrap_framl, text="页数:", font=('Xhei', '13')) \
        .place(x=370, y=60)
    page_num_chosen = ttk.Combobox(wrap_framl, width=10, textvariable=page_num)
    page_num_chosen['values'] = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
    page_num_chosen.current(0)
    page_num_chosen.place(x=425, y=60)
    search_btn = Button(wrap_framl, text='搜索', font=('Xhei', '13'), bg='red', fg='white',
                        command=search_job) \
        .place(x=540, y=55)
    clear_data_btn = Button(wrap_framl, text='清除', font=('Xhei', '13'), bg='red', fg='white', command=clear_data) \
        .place(x=600, y=55)
    other_keyword_label = Label(win, text="其他关键词", font=('Xhei', '14'), bg='red', fg='white')\
        .place(x=290, y=130)
    wrap_fram2 = LabelFrame(win, text="公司性质", width=200, height=570, font=('Xhei', '14')) \
        .place(x=20, y=170)
    company_type = [('所有', '99'), ('国企', '04'), ('外资(欧美)', '01'), ('外资(非欧美)', '02'), ('上市公司', '10'),
                        ('合资', '03'), ('民营', '05'), ('外企代表处', '06'), ('政府机关', '07'), ('事业单位', '08')]

    i = 0
    for com_type, com_type_num in company_type:
        Radiobutton(wrap_fram2, text=com_type, value=com_type_num, variable=v, font=('XHei', '13'))\
            .place(x=30, y=220+i)
        i += 50
    wrap_fram3 = LabelFrame(win, text="工资范围", width=200, height=570, font=('Xhei', '14')) \
        .place(x=245, y=170)
    wrap_fram4 = LabelFrame(win, text="学历要求", width=200, height=570, font=('Xhei', '14')) \
        .place(x=470, y=170)
    win.mainloop()


start_search()
```
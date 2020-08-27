---
title: 拉勾网职位爬取
date: 2020-02-10 10:44:01
tags: Python,爬虫
categories: Python
---

<center>一个爬取拉勾网职位信息简单程序，涉及到异步加载，使用json分析数据并保存到MongoDB数据库</center>

<!--more-->

```python
import requests
import json
import time
import pymongo
client = pymongo.MongoClient('127.0.0.1', 27017)
mydb = client['jobs']
lagou = mydb['lagou']

headers = {
    'Accept': 'application/json, text/javascript, */*; q=0.01',
    'Accept-Encoding': 'gzip, deflate, br',
    'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8',
    'Connection': 'keep-alive',
    'Content-Length': '37',
    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
    'Cookie': '_ga=GA1.2.1069426576.1578732618;'
              ' index_location_city=%E5%85%A8%E5%9B%BD;'
              ' user_trace_token=20200111165020-251b21ca-0efa-4e3a-b959-93855c309d4d;'
              ' lagou_utm_source=B;'
              ' JSESSIONID=ABAAAECABBJAAGI5F960B86A5C58E135C92D5813AEC82AC;'
              ' WEBTJ-ID=20200202115956-170040e12e86-02f47fddf1f558-b383f66-2073600-170040e12e96f0;'
              ' X_MIDDLE_TOKEN=dbdf524e8683f15975fa5fde4d8f2f39;'
              ' X_HTTP_TOKEN=669a21470347f2436986360851ac25caf63ec40c85;'
              ' _gat=1;'
              ' SEARCH_ID=f04d4617085a4104bf037670fd36f76c',
    'Host': 'www.lagou.com',
    'Origin': 'https://www.lagou.com',
    'Referer': 'https://www.lagou.com/jobs/list_java/',
    'Sec-Fetch-Mode': 'cors',
    'Sec-Fetch-Site': 'same-origin',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)' \
                  ' Chrome/79.0.3945.130 Safari/537.36',
    'X-Anit-Forge-Code': '0',
    'X-Anit-Forge-Token': 'None',
    'X-Requested-With': 'XMLHttpRequest'
}


def get_page(url, params):
    html = requests.post(url, data=params, headers=headers)
    print(html.text)
    json_data = json.loads(html.text)
    total_count = json_data['content']['positionResult']['totalCount']
    page_number = int(total_count/15) if int(total_count/15) < 30 else 30
    get_info(url, page_number)


def get_info(url, page):
    for pn in range(1, page+1):
        params = {
            'first': 'true',
            'pn': str(pn),
            'kd': '游戏'
        }
        try:
            html = requests.post(url, data=params, headers=headers)
            json_data = json.loads(html.text)
            results = json_data['content']['positionResult']['result']
            for result in results:
                infos = {
                    'businessZones': result['businessZones'],
                    'city': result['city'],
                    'district': result['district'],
                    'companyFullName': result['companyFullName'],
                    'companyShortName': result['companyShortName'],
                    'companySize': result['companySize'],
                    'industryField': result['industryField'],
                    'financeStage': result['financeStage'],
                    'firstType': result['firstType'],
                    'secondType': result['secondType'],
                    'thirdType': result['thirdType'],
                    'companyLabelList': result['companyLabelList'],
                    'salary': result['salary'],
                    'workYear': result['workYear'],
                    'education': result['education'],
                    'positionName': result['positionName'],
                }
                lagou.insert_one(infos)
                time.sleep(2)
        except KeyError:
            print("关键词错误")
            pass


if __name__ == '__main__':
    url = 'https://www.lagou.com/jobs/positionAjax.json'
    params = {
        'first': 'true',
        'pn': '1',
        'kd': 'java'
    }
    get_page(url, params)
    time.sleep(0.5)

```
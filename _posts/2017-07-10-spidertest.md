---
layout: post
title: 实例操作第一篇：利用requests和lxml进行简单爬虫实例
categories: 网络爬虫
description: 网络爬虫学习
keywords: Python, requests，lxml
---

# 实例操作第一篇：利用requests和lxml进行简单爬虫实例 #

> 声明：此实例仅作为技术学习参考，不得用于任务商业目的
>
> 转贴请注明本博客及作者

## 一、首先F12分析网页结构，查找请求信息

1、分析提取目标，确定好要提取的文本
![url](/images/blog/2017-07-10_1.png)

2、找到请求地址
![reurl](/images/blog/2017-07-10_2.png)

3、分析出传递参数
![urlarr](/images/blog/2017-07-10_3.png)

4、设置headers
![urlheader](/images/blog/2017-07-10_4.png)

## 二、开始提取文本操作
```python
'''引入工具包'''
import requests
from lxml import etree
import pandas as pd
import numpy as np
```


```python
'''F12分析request网页，建立url,headers,params'''
url = 'http://beijing.haodai.com/s4-10x12-0x0x9999/'  #请求页设置
headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36'}
params = {'p':3} #页码参数设置
```


```python
'''创建请求对象'''
'''这个例子的请求方式是GET，注意！！参数变量为params,与POST的不同'''
r = requests.get('http://beijing.haodai.com/s4-10x12-0x0x9999/',params = params,headers = headers)
html = str(r.content,encoding = 'utf-8')
```


```python
'''创建解析对象'''
xml = etree.HTML(html)
```


```python
'''开始提取文本'''
'''银行名称'''
banklist = []
for i in xml.xpath('//p[@class="co3 jqtap"]/a'):
    banklist.append(i.text)
print(len(banklist))
print(banklist)
```

    10
    ['宜信普惠-宜人贷', '宜信普惠-宜人精英贷', '宜信普惠-宜房贷', '宜信普惠-精英贷', '宁波银行-白领通', '中信银行-信金宝', '南京银行-诚易贷', '邮政储蓄银行-综合消费贷款', '平安易贷-薪金贷', '融联伟业-短期周转质押']



```python
'''产品类型'''
prolist = []
for i in xml.xpath('//p[@class="jqtap co9"]/a'):
    prolist.append(i.text)
print(len(prolist))
print(prolist)
```

    10
    ['信用贷', '信用贷', '抵押贷', '信用贷', '信用贷', '信用贷', '信用贷', '抵押贷', '信用贷', '抵押贷']



```python
'''贷款费用'''
prilist = []
for i in xml.xpath('//div[@class="wan co6"]/p/span'):
    prilist.append(i.text)
print(len(prilist))
print(prilist)
```

    10
    ['2.27万', '0.94万', '3万', '2.27万', '1.38万', '1.74万', '1.4万', '0.39万', '2.4万', '1.2万']



```python
'''贷款额度'''
linelist = []
for i in xml.xpath('//div[@class="wan co6"]/p/text()'):
    linelist.append(i.replace(' ','').replace('/',''))
print(len(linelist))
print(linelist)
```

    10
    ['10万', '10万', '10万', '10万', '10万', '10万', '10万', '10万', '10万', '10万']



```python
'''参考月利率'''
interlist = []
for i in xml.xpath('//div[@class="shmb"]/p[1]'):
    interlist.append(i.text)
print(len(interlist))
print(interlist)
```

    10
    ['参考月利率为0%', '参考月利率为0%', '参考月利率为0%', '参考月利率为0%', '参考月利率为0.5%', '参考月利率为0.6%', '参考月利率为0.6%', '参考月利率为0.6%', '参考月利率为0%', '参考月利率为0%']



```python
'''贷款管理费'''
feelist = []
for i in xml.xpath('//div[@class="shmb"]/p[2]'):
    feelist.append(i.text)
print(len(feelist))
print(feelist)
```

    9
    ['每月另收贷款额的1.89%为管理费', '每月另收贷款额的0.78%为管理费', '每月另收贷款额的2.5%为管理费', '每月另收贷款额的1.89%为管理费', '每月另收贷款额的0.65%为管理费', '每月另收贷款额的0.85%为管理费', '每月另收贷款额的0.57%为管理费', '每月另收贷款额的2%为管理费', '每月另收贷款额的1%为管理费']



```python
'''月供详情'''
mothlist = []
for i in xml.xpath('//div[@class="con03"]/p[1]'):
    mothlist.append(i.xpath('string(.)'))
print(len(mothlist))
print(mothlist)
```

    10
    ['分期还款10223元/月', '分期还款9113元/月', '到期还款', '分期还款10223元/月', '分期还款9483元/月', '分期还款9783元/月', '分期还款9503元/月', '分期还款8662元/月', '分期还款10333元/月', '分期还款9333元/月']



```python
'''月管理费'''
feelist = []
for i in xml.xpath('//p[@class="co6 jqtap"][2]'):
    feelist.append(i.text)
print(len(feelist))
print(feelist)
```

    10
    ['月管理费1.89%', '月管理费0.78%', '月管理费2.5%', '月管理费1.89%', '月利率0.5%', '月利率0.6%', '月利率0.6%', '月利率0.6%', '月管理费2%', '月管理费1%']



```python
'''贷款要求'''
reqlist = []
for i in xml.xpath('//div[@class="con04"]'):
    a = [j.text for j in i.findall('p[@class="co9 jqtap"]')]
    a = ''.join(a)
    reqlist.append(a)
reqlist = np.array(reqlist)
print(reqlist.shape)
print(reqlist)
```

    (10,)
    ['22-55周岁工作满半年必须打卡工资4000' '22-60周岁税后收入3000以上工作满6个月' '年龄18-65周岁本地有商品房信用良好'
     '22-60周岁税后收入3000以上工作满6个月' '25-60周岁月收入4000元以上非本地户籍需有房产'
     '22-65周岁本地工作6个月以上银行代发工资' '18-65周岁北京工作满半年月收入5000以上'
     '\u200b有北京本地商品房房龄不超过20年估值50万以上' '21-55周岁工作满6个月信用记录良好' '22-65周岁有抵押物信用良好']



```python
'''产品特点'''
textlist = []
for i in xml.xpath('//div[@class="con05"]//ul[@class="icon"]'):
    a = [ j.attrib.get('title') for j in i.findall('li/a')]
    a = ''.join(a)
    textlist.append(a)
textlist = np.array(textlist)
print(len(textlist))
textlist
```

    10

    array(['放贷速度快利率较低', '当天放款放贷额度高放贷速度快', '放贷速度快需本地房产', '当天放款放贷额度高放贷速度快',
           '3天放款仅特定人群利率较低', '工资要求高利率较低', '仅特定人群上门服务手续简便', '10年授信需本地房产',
           '放贷速度快上门服务手续简便', '放贷速度快手续简便'],
          dtype='<U14')



## 三、创建数据表
```python
'''设置数据表名和数据字段'''
name = ['银行名称','产品类型','贷款费用','贷款额度','参考月利率',
        '月供详情','月管理费','贷款要求','产品特点']
data = [banklist,prolist,prilist,linelist,interlist,mothlist,feelist,reqlist,textlist]
```


```python
'''创建数据表'''
df = pd.DataFrame(columns=name)
for (i,j) in zip(name,data):
    df[i] = j
df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>银行名称</th>
      <th>产品类型</th>
      <th>贷款费用</th>
      <th>贷款额度</th>
      <th>参考月利率</th>
      <th>月供详情</th>
      <th>月管理费</th>
      <th>贷款要求</th>
      <th>产品特点</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>宜信普惠-宜人贷</td>
      <td>信用贷</td>
      <td>2.27万</td>
      <td>10万</td>
      <td>参考月利率为0%</td>
      <td>分期还款10223元/月</td>
      <td>月管理费1.89%</td>
      <td>22-55周岁工作满半年必须打卡工资4000</td>
      <td>放贷速度快利率较低</td>
    </tr>
    <tr>
      <th>1</th>
      <td>宜信普惠-宜人精英贷</td>
      <td>信用贷</td>
      <td>0.94万</td>
      <td>10万</td>
      <td>参考月利率为0%</td>
      <td>分期还款9113元/月</td>
      <td>月管理费0.78%</td>
      <td>22-60周岁税后收入3000以上工作满6个月</td>
      <td>当天放款放贷额度高放贷速度快</td>
    </tr>
    <tr>
      <th>2</th>
      <td>宜信普惠-宜房贷</td>
      <td>抵押贷</td>
      <td>3万</td>
      <td>10万</td>
      <td>参考月利率为0%</td>
      <td>到期还款</td>
      <td>月管理费2.5%</td>
      <td>年龄18-65周岁本地有商品房信用良好</td>
      <td>放贷速度快需本地房产</td>
    </tr>
    <tr>
      <th>3</th>
      <td>宜信普惠-精英贷</td>
      <td>信用贷</td>
      <td>2.27万</td>
      <td>10万</td>
      <td>参考月利率为0%</td>
      <td>分期还款10223元/月</td>
      <td>月管理费1.89%</td>
      <td>22-60周岁税后收入3000以上工作满6个月</td>
      <td>当天放款放贷额度高放贷速度快</td>
    </tr>
    <tr>
      <th>4</th>
      <td>宁波银行-白领通</td>
      <td>信用贷</td>
      <td>1.38万</td>
      <td>10万</td>
      <td>参考月利率为0.5%</td>
      <td>分期还款9483元/月</td>
      <td>月利率0.5%</td>
      <td>25-60周岁月收入4000元以上非本地户籍需有房产</td>
      <td>3天放款仅特定人群利率较低</td>
    </tr>
  </tbody>
</table>
</div>

为了感受XML的功能，本案例并未对数据列进行深度加工，

>## 这个爬虫很简单，在多页提取方面可以改进，有空会改进下试试

---
layout: post
title: 网络爬虫第一篇：网络爬虫基础和Requests库
categories: Blog
description: 网络爬虫学习
keywords: Python, Requests，Web
---

# 网络爬虫第一篇：网络爬虫基础和Requests库 #
----------
## 关于网络爬虫基础 ##
一直都想好好研究，可惜时间利用不好（懒癌患者....）
岁数大了记性不好，还是写个笔记免得都忘了，水平有限，诸位见谅

网络爬虫（web crawber）又叫网络蜘蛛，即采集web上的信息。
常见的爬虫主要使用如下技术：

> `python3：基础语言库`

> `Requests库:html请求处理库`

> `BeautifulSoup库简称bs4:html解析库`

> `lxml库:更强的html解析库`

> `re库:正则表达式库`

> `Scrapy库：霸气的爬虫框架库`

## 关于python版本 ##
现代技术更新快，为了避免头疼的utf-8，建议还是用py3吧，目前到3.5版本了。
而且Scrapy也支持了还有啥好担心的呢

## 关于Requests库 ##
下面开始重点记录下这个包的功能
>这个包类似于python标准库urllib2，用于网站请求处理 功能强大，使用简便
>
> 分享：[Requests中文文档](http://www.zhidaow.com/post/python-requests-install-and-brief-introduction)

## 1. 首先说下WEB请求方式
> get方式：一般方式，无需登录的网站使用
> Post方法：表单方式，需要登录的网站使用

## 2. Get或Post的使用方法

### 首次查看登录页面form的action属性，确定Get或Post的网址
> 例如：主页为http://X.X.X.X/
> ```HTML
> <form name="LoginForm"
> 	  method="POST"
>       action="/ODWEKWeb/ODLogon"
>       onsubmit="return docheckform()"
> >
> ```
> Post网址为http://X.X.X.X/ODWEKWeb/ODLogon

### 其次查看input的name属性，确定ID和密码的变量名称
>例如：
> ```HTML
> <input type="text"
> 	   name="_user"
> 	   value=""
> >
> <input type="password"
> 	   name="_password"
> 	   value="">
> ```
> ID的变量名为_user
> 密码的变量名_password

### 最简单方法:使用浏览器F12开发者模式查看
1、打开浏览器，进入登录界面后，按F12，选择Network进行监控，然后输入ID和密码，点击登录按钮，监控收发包。
2、可查信息如下：
![123](/images/blog/2017-07-06_1_1.png)

![123](/images/blog/2017-07-06_1_2.png)

![123](/images/blog/2017-07-06_1_3.png)

![123](/images/blog/2017-07-06_1_4.png)
### 普通网页登录
需登记网站涉及cookies，建议多使用session对象，避免一系统麻烦
例如：
> #1、导入包
```python
import requests
from requests import session
session = session()
headers = {'user-agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36'}
```
> #2、带cookies参数
```python
params = {'_user':'02818wd','_password':'pass2012'}
r = requests.post('http://X.X.X.X/ODWEKWeb/ODLogon',data = params,headers = headers)
r = requests.get('http://X.X.X.X/ODWEKWeb/modifypassword.jsp',cookies = r.cookies,headers = headers)
print(r.text)#输出内容
```
> #3、自动追踪cookies，无需带cooki参数
```python
params = {'_user':'02818wd','_password':'pass2012'}
s = session.post('http://X.X.X.X/ODWEKWeb/ODLogon',data = params,headers = headers)
s = session.get('http://X.X.X.X/ODWEKWeb/modifypassword.jsp',headers = headers)
print(s.text)
```
```python
params = {'folderName':'VIEW_DGMSRPT','type':'true'}
s = session.post('http://X.X.X.X/ODWEKWeb/ODGetCriteria',data = params,headers = headers)
print(s.text)
```

### 登录HTTP认证网页
在登录HTTP认证网页时，可使用auth模块
> 导入auth
```python
import requests
from requests.auth import AuthBase
from requests.auth import HTTPBasicAuth
auth = HTTPBasicAuth('ID','PASSWORD')
r = requests.post('http://X.X.X.X/ODWEKWeb/ODLogon',auth = auth)
print(r.text)
```

## 3. 解释器准备数据
在使用bs4，或lxml等解释器前，准备好数据，注意编码方式
>例如：
```python
#抓取并保存页面信息
r=requests.get('http://X.X.X.X/',headers=headers)
html=r.content
#对抓取的页面进行编码
html=str(html, encoding = "GBK")
```

本篇记录了爬虫基础知识，以及Requests库使用，接下来将开始网页解释器学习
>### 文章及图片归 神烦DMer所有。欢迎转载，请注明转自“神烦DMer博客”

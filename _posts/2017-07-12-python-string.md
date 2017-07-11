---
layout: post
title: 特别篇：关于python解决字符串去除全部标点符号的方法
categories: python
description: python
keywords: Python, 字符串，string
---

# python解决字符串去除全部标点符号的方法 #

> 遇到去除字符串中所有标点符号的问题，想到的解决方法:

>不用FOR循环，不用正则表达式


```python
'''引入string模块'''
import string
```


```python
'''使用标点符号常量'''
string.punctuation
```


```python
text = "*/@》--【】--12()测试*()"

'''去除字符串中所有的字符，可增加自定义字符'''
def strclear(text,newsign=''):
    import string # 引入string模块
    signtext = string.punctuation + newsign # 引入英文符号常量，可附加自定义字符，默认为空
    signrepl = '@'*len(signtext) # 引入符号列表长度的替换字符
    signtable = str.maketrans(signtext,signrepl) # 生成替换字符表
    return text.translate(signtable).replace('@','') # 最后将替换字符替换为空即可

strclear(text,'》【】')
```




    '12测试'

> 声明：转载请联系博主，并注明本博客及作者

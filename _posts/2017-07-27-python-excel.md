---
layout: post
title: 关于python3中利用os和pandas来合并当前目录下所有excel文件的方法
categories: Python
description: python3中如何合并excel文件
keywords: Python, OS，pandas，excel,文件
---

# 关于python3中利用os和pandas来合并当前目录下所有excel文件的方法 #

前面写了一篇关于如何读取当前路径下所有文件路径的方法，今天继续延伸做个合并excel的方法笔记

受不了网上那些xlrd、xlsxwriter或者xlwt这三个模块的复杂写法，我还是发挥自己又懒又急躁的“特长”，把注意力转移到pandas这个模块上来了

应该算是很容易实现的，同时可以利用pandas的数据处理自动化特点来消除很多数据格式不规范的情况，比如文本型的123，自动转为数值型123。另外这个方法可处理各种数据列数量不一致、数据缺失等情况，案例数据如下图：
![text](/images/blog/2017-07-27_0.png)

### 1、引入模块


```python
import pandas as pd
import os
import x
```

### 2、取出指定目录下的全部excel文件路径


```python
path = r'C:\Users\JAY23\Desktop\测试'
dirlist = []
for dirpath,dirname,filename in os.walk(path):
    for i in filename:
        dirlist.append(os.path.join(dirpath,i))
dirlist
```




    ['C:\\Users\\JAY23\\Desktop\\测试\\0-1.xlsx',
     'C:\\Users\\JAY23\\Desktop\\测试\\0-2.xlsx',
     'C:\\Users\\JAY23\\Desktop\\测试\\文件夹1\\1-1.xlsx',
     'C:\\Users\\JAY23\\Desktop\\测试\\文件夹1\\1-2.xlsx',
     'C:\\Users\\JAY23\\Desktop\\测试\\文件夹2\\2-1.xlsx',
     'C:\\Users\\JAY23\\Desktop\\测试\\文件夹2\\2-2.xlsx']



### 3、创建一个df对象列表，并进行合并操作


```python
dflist = []
for i in dirlist:
    dflist.append(pd.read_excel(i))
```

### 4、利用pd.concat函数来合并excel,涉及excel数据列数量不一致的、有空值等情况均没有影响合并效果


```python
mydf = pd.concat(dflist)
```

### 5、导出合并后的excel，因为此方法合并后index存在重复，可选择去除index


```python
mydf.to_excel('mydf.xlsx',index=None)
```

### 6、查看合并后的excel文件


```python
pd.read_excel('mydf.xlsx')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>体重</th>
      <th>姓名</th>
      <th>年龄</th>
      <th>性别</th>
      <th>身高</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>NaN</td>
      <td>张三</td>
      <td>20.0</td>
      <td>男</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>李四</td>
      <td>25.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>王五</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>赵六</td>
      <td>30.0</td>
      <td>女</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>80.0</td>
      <td>杨子</td>
      <td>22.0</td>
      <td>男</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>李子</td>
      <td>35.0</td>
      <td>女</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### 7、可查看合并后数据类型情况


```python
aa = pd.read_excel('mydf.xlsx')
aa.dtypes
```




    体重    float64
    姓名     object
    年龄    float64
    性别     object
    身高    float64
    dtype: object

>### 声明：作者原创文章，未经同意严禁转载

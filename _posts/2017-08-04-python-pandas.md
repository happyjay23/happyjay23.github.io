---
layout: post
title: Scarpy特别篇：关于python3中pandas通过字典创建dataframe来解决scrapy爬虫数据保存
categories: 网络爬虫
description: 网络爬虫学习
keywords: Python, Scarpy，框架，爬虫，pandas,pipeline
---

想了好多办法能让爬虫数据保存省心省事，考虑到item本身就是字典结构，所以我研究了下利用pandas中将itme字典创建成dataframe数据表，一旦能把爬虫数据保存进pandas，那么保存生什么csv、xls等格式就变得简单了

感觉很好用就写下来了，其中关于字典为元素的列表形式非常关键!


```python
import pandas as pd
```

### 1、利用嵌套列表形式生产数据表


```python
pd.DataFrame([[1,1,1],[2,2,2]])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



### 2、利用字典嵌套列表形式创建数据表


```python
pd.DataFrame({'a':[1,1,1],'b':[2,2,2]})
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



### 3、利用列表嵌套字典形式创建数据表，关键是列表里元素是字典！！


```python
df = pd.DataFrame([{'a':1,'b':1},{'a':2,'b':2}])
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
item = {'b':3,"a":3}
df.append([item])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



若在scrapy的pipeline中把item一个一个都装进dataframe里，剩下的事情变得美妙多了，毕竟pandas导出各种格式的数据表实在so easy

好了有了这个基础，下一篇我会实际测试一下效果如何

> ### 声明：本人原创文章，未经同意严禁转载

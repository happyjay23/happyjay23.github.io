---
layout: post
title: 数据分析特别篇：关于Pandas.Series和numpy.array的区别和联系
categories: 数据分析
description: 数据分析学习
keywords: Python, Pandas，numpy，数组
---
# 关于Pandas.Series和numpy.array的区别和联系 #
突然感觉学习让人烦的不是内容困难，而是时不时的发现之前认为理所应当的地方，其实是错误的,这很让人沮丧~~~

当然了，这也是内功不断提高的必然过程，哈哈哈

## 比如下面这个关于pandas series的理解，就非常不对劲了！

>  这是我做个爬虫时遇到问题，当时抓出来的数据有一列数据是这样的：张三/[2017-01-21],而且输出了DataFrame。需要把这列拆分为姓名和日期两个数据列，强迫症的我又不想回头去处理原始文本数据，就想在DataFrame做手脚.

>我看过的书和教程都没提到这个，网上搜了半天也没提到什么直接的方法能做到，搞了半天终于弄出来了，所以赶紧写下来不然回头又忘记了


```python
import pandas as pd
import numpy as np
```

## 我们给三个series的例子 ##


```python
'''第一个例子：元素为数值类型int'''
pd.Series([1,1]).values
```




    array([1, 1], dtype=int64)




```python
'''查看series数组的维度'''
pd.Series([1,1]).values.shape
```




    (2,)




```python
'''第二个例子：元素为python对象类型list'''
pd.Series([[1,1,1],[2,2,2]]).values
```




    array([[1, 1, 1], [2, 2, 2]], dtype=object)




```python
'''查看series数组的维度'''
pd.Series([[1,1,1],[2,2,2]]).values.shape
```




    (2,)




```python
'''将series数组转为numpy.array数组的类型'''
np.array(pd.Series([[1,1,1],[2,2,2]]))
```




    array([[1, 1, 1], [2, 2, 2]], dtype=object)




```python
'''将series数组转为numpy.array数组的维度'''
np.array(pd.Series([[1,1,1],[2,2,2]])).shape
```




    (2,)




```python
'''第三个例子：输入二维数组'''
pd.Series(np.array([[1,1,1],[2,2,2]])).values
```


    ---------------------------------------------------------------------------

    Exception       Traceback (most recent call last)


    Exception: Data must be 1-dimensional


## 个人总结pd.series的特性： ##
> 1、pd.series将输入数据的维度视为一维，不支持多维array！

> 2、无论是输出的series还是将输出的series转换成numpy数组，维度保持一维！

> 3、pd.series输出自动判断是否转为object类型

## 我们再来看3个np.array的例子 ##


```python
'''第一个例子：元素的类型相同'''
np.array([['a', 'a', 'a'], ['b','b','b']])
```




    array([['a', 'a', 'a'],
           ['b', 'b', 'b']],
          dtype='<U1')




```python
'''查看维度'''
np.array([['a', 'a', 'a'], ['b','b','b']]).shape
```




    (2, 3)




```python
'''第二个例子：元素为python对象类型list'''
np.array([[1,1,1], [2,2,2]])
```




    array([[1, 1, 1],
           [2, 2, 2]])




```python
'''查看维度'''
np.array([[1,1,1], [2,2,2]]).shape
```




    (2, 3)




```python
'''第三个例子：元素的类型不同，包括列表和int'''
np.array([[[1], [1], [1]], [2,2,2]])
```




    array([[[1], [1], [1]],
           [2, 2, 2]], dtype=object)




```python
'''查看维度'''
np.array([[[1], [1], [1]], [2,2,2]]).shape
```




    (2, 3)



## 个人总结np.array的特性： ##
> 1、np.array将输入数据视为多维，每行长度一致即可
>
> 2、np.array输出数据的维度保持多维
>
> 3、np.array把不同类型数据转为object类型

## 重要结论： ##
分别看series和np.array的第二例子！！列表list作为输入数据，series和np.array两者得到结果完全不同

> pd.Series：将输入list视为多维，观察最底层元素的数据类型来判断输出的类型，且保持维度多维

> np.array：将输入list视为一维，观察最外层元素的数据类型来判断输出的类型，且保持维度一维

> list: 核心作用，一维series和多维np.array两者通过.tolist()或内置函数list()转化为list后而进行间接转化

![qubie](/images/blog/2017-07-11_1.png)
![lianxi](/images/blog/2017-07-11_2.png)

```python
'''证明：维度不同的series与np.array相互转化,利用.tolist()函数'''
List = [[1,1,1], [2,2,2]]
Sr = pd.Series(List)
print('pd.Series的维度{0}'.format(Sr.shape))
Nr = np.array(List)
print('nd.array的维度{0}'.format(Nr.shape))

print('pd.Series是否能变回原list：{0}'.format(List == Sr.tolist()))
print('np.array是否能变回原list：{0}'.format(List == Nr.tolist()))
```

    pd.Series的维度(2,)
    nd.array的维度(2, 3)
    pd.Series是否能变回原list：True
    np.array是否能变回原list：True


## 说了这么多，到底有用吗，我遇到的问题：某一列实际包括了2种数据维度，举个例子吧 ##


```python
'''例子：将数据列age&sex包括2个特征维度，需要拆分为二列数据'''
df = pd.DataFrame([['Tom','18|男'],['Joho','20|女'],['Tim','13|女']],columns=['name','age&sex'])
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age&amp;sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Tom</td>
      <td>18|男</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Joho</td>
      <td>20|女</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Tim</td>
      <td>13|女</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''可以看出series的维度就是一维，而我们需要二维'''
df['age&sex'].values.shape
```




    (3,)




```python
'''进行字符串分割后.虽然看起来像二维'''
df['age&sex'].str.split('|').values
```




    array([['18', '男'], ['20', '女'], ['13', '女']], dtype=object)




```python
'''再来查看维度还是一维,怎么办？？'''
df['age&sex'].str.split('|').shape
```




    (3,)




```python
'''这回我们调用tolist函数转为list,或者内置函数list()'''
List = df['age&sex'].str.split('|').tolist()
List
```




    [['18', '男'], ['20', '女'], ['13', '女']]




```python
'''新增数据列，并赋值,目标达到！'''
df['age'],df['sex'] = pd.Series(),pd.Series()
df[['age','sex']] = List
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>age&amp;sex</th>
      <th>age</th>
      <th>sex</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Tom</td>
      <td>18|男</td>
      <td>18</td>
      <td>男</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Joho</td>
      <td>20|女</td>
      <td>20</td>
      <td>女</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Tim</td>
      <td>13|女</td>
      <td>13</td>
      <td>女</td>
    </tr>
  </tbody>
</table>
</div>



如果网友有其他方法，非常欢迎分享！，人生苦短用Python！

> ### 声明：转贴请联系作者，并注明转载本博客及作者版权

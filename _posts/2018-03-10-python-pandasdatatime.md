---
layout: post
title: 关于pandas中时间序列的基本操作说明
categories: Python
description: pandas中的resample分组操作和idmax函数
keywords: Python, pandas，分组操作，时间序列
---
终于决定转入数据分析行业了，感觉自己内心终于释放了，无论结果如何，满心欢喜啊

因为前期的工作原因，耽搁了半年的blog又开始了，哈哈！

今天练习关于时间序列数据的基本操作，由于时间序列数据的特点，需要进行重采样和基本统计函数的应用，这块不是很熟悉，记录大致过程日后使用

### 一.首先将数据列的格式应为时间序列格式

例如：time列为int格式，需要通过`pd.to_datatime`函数转为时间数据
```python
df['time'] = pd.to_datatime(df['time'],format='%Y')
#注意时间序列的格式

```

### 二、其次需要时间序列数据设置为index索引

例如将数据列time转为index，需要通过`df.set_index`函数完成
```python
df = df.set_index('time',drop = True)
#将time列转为index，同时去掉该列作为特征数据列
```

### 三、最后使用简便函数resample，进行分组聚合运算等操作

用于各类采样操作同时可进行分组操作，不同于groupby关注特征值的分组操作
它重点使用在时间序列作为index的数据中，通过对index的分组操作来聚合运算

例如求频率为10年的数据求和,其中特征num求最大值
```python
df2 = df.resample('10AS').sum()
df2.num = df.resample('10AS')['num'].max()
df2
```

其中时间偏移量参数预设值包括如下：
![canshu](/images/blog/2018-03-10_0.png)

### 四、可利用index来寻找极值数据

例如求哪一年的特殊数据最大或最小，运用`idxmax`等函数
```python
df2.idxmax()
#直接返回每列最大数据所在的时间索引值
```

### 总结
1、时间序列数据涉及格式转化、升降重采样，频率转化等

2、主要应用在时间序列分析中，后面还会继续加强这方面学习

> ## 原创内容未经作者同意，严禁转载

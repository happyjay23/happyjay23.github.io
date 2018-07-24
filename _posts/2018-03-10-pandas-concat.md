---
layout: post
title: 关于pandas中数据表的合并和重塑
categories: Pandas
description: pandas中的concat、merge、join三个函数进行合并和重塑
keywords: Python, pandas，合并，拼接
---

today继续练习pandas操作，主要是不同数据组的合并和拼接

### 1. 轴向合并数据：concat和对象方法append
一般是全量合并操作，可设置是否交集等，可用于`N多张合并`！
```python
data1 = {'a':[1,2],'b':[1,2]}
data2 = {'a':[3,4],'b':[3,4]}
pd.concat(data1,data2)
#按行轴纵向合并
pd.concat(data1,data2,axis=1)
#按列轴横向合并
data1.append(data2)
#利用对象方法也可以实现
```
### 2.列主键合并：merge
一般是特征列中有相同的特征列可进行合并索引使用，该函数的主要 应用场景是针对同一个主键存在两张包含不同特征的表，通过该主键的连接，将两张表进行合并。合并之后，两张表的行数没有增加，列数是两张表的列数之和减一。
```python
data1 = {'a':[1,2],'b':[1,2]}
data2 = {'a':[1,2],'c':[3,4]}
pd.merge(data1,data2,on='a',how='inner')
#顶级函数，利用主键a进行合并
data1.merge(data2,on='a',how='inner')
#利用对象方法同样也可以实现
#当两个表主键名称不同时，也可以实现
pd.merge(data1,data2,left_on = 'a',right_on = 'aa')
```
### 3.索引合并：join
DataFrame的join实例方法，是为了方便实现索引合并

注意！join可以合并N个表，而merge只能合并两张表
```python
data1.join([data2,data3],how='inner')
```

### 总结
1、合并和重塑属于数据规整范畴

2、注意数据保留情况，以及合并主键选择问题

> ## 原创内容未经作者同意，严禁转载

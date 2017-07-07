---
layout: post
title: 数据分析第一篇：Pandas基础
categories: 数据分析
description: 数据分析学习
keywords: Python, Pandas，numpy
---

# 数据分析第一篇：Pandas基础 #

```python
import numpy as np
import pandas as pd
import matplotlib as plt
%matplotlib inline
```

# 数据导出导入


```python
'''1、创建数据表'''
df = pd.DataFrame({"id":[1001,1002,1003,1004,1005,1006],
                   "date":pd.date_range('20130102', periods=6),
                   "city":['Beijing ', 'SH', ' guangzhou ', 'Shenzhen', 'shanghai', 'BEIJING '],
                   "age":[23,44,54,32,34,32],
                   "category":['100-A','100-B','110-A','110-C','210-A','130-F'],
                   "price":[1200,np.nan,2133,5433,np.nan,4432]},
                  columns =['id','date','city','category','age','price'])
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>Beijing</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>guangzhou</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>Shenzhen</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>shanghai</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''2、导出数据表'''
# df.to_excel('pandas_test.xls')
df.to_csv('pandas_test.csv')

```


```python
'''3、导入数据表'''
df = pd.read_csv('pandas_test.csv',index_col=1)#可指定索引
df = pd.read_excel('pandas_test.xls',header=0)#可指定表头
```

# 数据查看


```python
'''1、数据维度：数据由几行几列'''
df.shape
```




    (6, 6)




```python
'''2、数据信息：数据维度、列名称、数据格式和所占空间等'''
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 6 entries, 0 to 5
    Data columns (total 6 columns):
    id          6 non-null int64
    date        6 non-null datetime64[ns]
    city        6 non-null object
    category    6 non-null object
    age         6 non-null int64
    price       4 non-null float64
    dtypes: datetime64[ns](1), float64(1), int64(2), object(2)
    memory usage: 288.0+ bytes



```python
'''3、数据格式'''
df.dtypes
```




    id                   int64
    date        datetime64[ns]
    city                object
    category            object
    age                  int64
    price              float64
    dtype: object




```python
'''4、查看空值，可以对整个数据表，也可以单独对某一列进行空值检查'''
df.isnull()
df['price'].isnull()
df['price'].isnull().value_counts()#可查看到底有几个空值出现
df.isnull.sum() #直接查看所有特征是否存在空值
```




    0    False
    1     True
    2    False
    3    False
    4     True
    5    False
    Name: price, dtype: bool




```python
'''5、去重复查看所有唯一值，只能对数据表中的某一列进行，类似去重复功能'''
a = df['age']
a.unique()#两种方法
a.drop_duplicates()
```




    array([23, 44, 54, 32, 34], dtype=int64)




```python
'''6、查看数据值,不包含表头'''
df.values
```




    array([[1001, Timestamp('2013-01-02 00:00:00'), 'Beijing ', '100-A', 23,
            1200.0],
           [1002, Timestamp('2013-01-03 00:00:00'), 'SH', '100-B', 44, nan],
           [1003, Timestamp('2013-01-04 00:00:00'), ' guangzhou ', '110-A', 54,
            2133.0],
           [1004, Timestamp('2013-01-05 00:00:00'), 'Shenzhen', '110-C', 32,
            5433.0],
           [1005, Timestamp('2013-01-06 00:00:00'), 'shanghai', '210-A', 34,
            nan],
           [1006, Timestamp('2013-01-07 00:00:00'), 'BEIJING ', '130-F', 32,
            4432.0]], dtype=object)




```python
'''7、查看列名'''
df.columns
```




    Index(['id', 'date', 'city', 'category', 'age', 'price'], dtype='object')




```python
'''8、查看前几行数据,默认前10行'''
df.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>Beijing</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>guangzhou</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''9、查看后几行数据，默认后10行'''
df.tail(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>Shenzhen</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>shanghai</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
    </tr>
  </tbody>
</table>
</div>



# 数据清洗

一、处理空值


```python
'''1、删除空值'''
df.dropna(how='any')#参数任意位置为空
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>Beijing</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>guangzhou</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>Shenzhen</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#2、填充空值，用特定值如0或均值
df.fillna(df['price'].mean())#用均值填充
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>Beijing</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>3299.5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>guangzhou</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>Shenzhen</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>shanghai</td>
      <td>210-A</td>
      <td>34</td>
      <td>3299.5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
    </tr>
  </tbody>
</table>
</div>



二、处理空格


```python
#1、清理空格,清除字符中空格
df['city'] = df['city'].map(str.strip)
```




    0      BEIJING
    1           SH
    2    GUANGZHOU
    3     SHENZHEN
    4     SHANGHAI
    5      BEIJING
    Name: city, dtype: object



三、大写小写转化


```python
#1、大写转小写
df['city'].str.lower()#转小写
df['city'] =df['city'].str.upper()#转大写
```




    0      BEIJING
    1           SH
    2    GUANGZHOU
    3     SHENZHEN
    4     SHANGHAI
    5      BEIJING
    Name: city, dtype: object



四、改变数据格式


```python
#1、修改数据格式
df['age'].astype('int')
```




    0    23
    1    44
    2    54
    3    32
    4    34
    5    32
    Name: age, dtype: int32



五、更改列名称


```python
#1、更改列名称
df.rename(columns={'age':'age_new'})
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age_new</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
    </tr>
  </tbody>
</table>
</div>



六、删除重复值


```python
#1、删除重复值，可选择保留最先或者最后的值
df['city'].drop_duplicates()#默认
df['city'].drop_duplicates(keep='last')
```




    1             SH
    2     GUANGZHOU
    3       SHENZHEN
    4       SHANGHAI
    5       BEIJING
    Name: city, dtype: object



七、数据修改或替换


```python
'''非中文时可直接使用，中文时需.str.replace方可成功'''
df['city'].replace('SH','BEIJING')
```




    0      BEIJING
    1      BEIJING
    2    GUANGZHOU
    3     SHENZHEN
    4     SHANGHAI
    5      BEIJING
    Name: city, dtype: object



# 数据预处理


```python
df1=pd.DataFrame({"id":[1001,1002,1003,1004,1005,1006,1007,1008],
                  "gender":['male','female','male','female','male','female','male','female'],
                  "pay":['Y','N','Y','Y','N','Y','N','Y',],
                  "m-point":[10,12,20,40,40,40,30,20]})
```

一、数据合并


```python
#1、数据合并
df_new = pd.merge(df,df1,how='inner')#共有列名匹配合并，不匹配的丢弃
df_new = pd.merge(df,df1,how='left')#左合并，保留df
df_new = pd.merge(df,df1,how='right')#右合并，保留df1
df_new = pd.merge(df,df1,how='outer')#全合并，全部保留df，df1
```

二、设置索引列


```python
#1、将某列设置为索引，该列数据不能重复
df_new.set_index('id')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
    </tr>
    <tr>
      <th>id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1001</th>
      <td>2013-01-02</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1002</th>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
    </tr>
    <tr>
      <th>1003</th>
      <td>2013-01-04</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1004</th>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1005</th>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
    </tr>
    <tr>
      <th>1006</th>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
    </tr>
  </tbody>
</table>
</div>



三、排序


```python
#1、按索引排序
df_new.sort_index()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
    </tr>
  </tbody>
</table>
</div>




```python
#1、按数值排序
df_new = df_new.sort_values(by=['age','id'])#可设置多级排序
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
    </tr>
  </tbody>
</table>
</div>



#★★ 应添加数据列的新增、删除、修改
#应添加数据列划分，loc、iloc、ix的使用区别


```python
四、数据分组
```


```python
#1、单条件分组标记列
df_new['group'] = np.where(df_new['age'] > 40,'high','low')
df_new
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
      <td>low</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
      <td>high</td>
    </tr>
  </tbody>
</table>
</div>




```python
#2、多条件复合，生成分组标记列
df_new.loc[(df_new['age'] >= 40) & (df_new['price']>= 2000), 'sign']=1
df_new
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
      <td>low</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
      <td>high</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
五、数据分列
```


```python
# 1、依据分割符号对某个列进行分列,然后通过index与原df进行合并
split = pd.DataFrame([x.split('-') for x in df_new['category']],index=df_new.index,columns=['category','size'])
df_new = pd.merge(df_new,split,left_index=True,right_index=True)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>100</td>
      <td>A</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>110</td>
      <td>C</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
      <td>low</td>
      <td>NaN</td>
      <td>210</td>
      <td>A</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
      <td>100</td>
      <td>B</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
      <td>high</td>
      <td>1.0</td>
      <td>110</td>
      <td>A</td>
    </tr>
  </tbody>
</table>
</div>



# 数据提取和切片

一、按索引标签值进行提取：loc


```python
#1、按索引标签提取单行
df_new.loc[3]
```




    id                           1004
    date          2013-01-05 00:00:00
    city                     SHENZHEN
    category_x                  110-C
    age                            32
    price                        5433
    gender                     female
    m-point                        40
    pay                             Y
    group                         low
    sign                          NaN
    category_y                    110
    size                            C
    Name: 3, dtype: object




```python
#2、按索引标签提取多行
df_new.loc[3:4]#★★注意此处,是取标签值间的
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>110</td>
      <td>C</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
      <td>low</td>
      <td>NaN</td>
      <td>210</td>
      <td>A</td>
    </tr>
  </tbody>
</table>
</div>




```python
#3、恢复顺序索引
df_new.reset_index()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>id</th>
      <th>date</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>1001</td>
      <td>2013-01-02</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>100</td>
      <td>A</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>1004</td>
      <td>2013-01-05</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>110</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>1006</td>
      <td>2013-01-07</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1005</td>
      <td>2013-01-06</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
      <td>low</td>
      <td>NaN</td>
      <td>210</td>
      <td>A</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>1002</td>
      <td>2013-01-03</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
      <td>100</td>
      <td>B</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2</td>
      <td>1003</td>
      <td>2013-01-04</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
      <td>high</td>
      <td>1.0</td>
      <td>110</td>
      <td>A</td>
    </tr>
  </tbody>
</table>
</div>




```python
#4、设置日期为索引
df_new = df_new.set_index('date')
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>1001</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>100</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>1004</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>110</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2013-01-07</th>
      <td>1006</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>1005</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
      <td>low</td>
      <td>NaN</td>
      <td>210</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>1002</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
      <td>100</td>
      <td>B</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>1003</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
      <td>high</td>
      <td>1.0</td>
      <td>110</td>
      <td>A</td>
    </tr>
  </tbody>
</table>
</div>




```python
#5、按日期标签提取
df_new.loc[:'2013-01-03']
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>1001</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>100</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>1002</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
      <td>100</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>



二、按实际位置提取数据：iloc


```python
#1、按实际位置提取
df_new.iloc[0:2,1:4]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
    </tr>
    <tr>
      <th>2013-01-05</th>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
    </tr>
  </tbody>
</table>
</div>




```python
#2、按实际位置提取特定数据
df_new.iloc[[1,3,5],1:4]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-05</th>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
    </tr>
    <tr>
      <th>2013-01-06</th>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
    </tr>
  </tbody>
</table>
</div>



三、按标签和位置提取：ix


```python
#1、混合提取，规则：先按标签，再按位置
df_new.ix[:'2013-01-04',:3]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>1001</td>
      <td>BEIJING</td>
      <td>100-A</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>1002</td>
      <td>SH</td>
      <td>100-B</td>
    </tr>
    <tr>
      <th>2013-01-04</th>
      <td>1003</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
    </tr>
  </tbody>
</table>
</div>



四、按条件提取（值或区域）


```python
#1、直接逻辑条件提取
df_new[df_new['city'].isin(['SH','BEIJING'])]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>1001</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>100</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2013-01-07</th>
      <td>1006</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>1002</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
      <td>100</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>




```python
#2.嵌套函数逻辑值提取
df_new.loc[df_new['city'].isin(['SH','BEIJING'])]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>1001</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>100</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2013-01-07</th>
      <td>1006</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>1002</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
      <td>100</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>




```python
#3、嵌套函数逻辑值提取
df_new.ix[df_new['city'].isin(['SH','BEIJING'])]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
    <tr>
      <th>date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2013-01-02</th>
      <td>1001</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>100</td>
      <td>A</td>
    </tr>
    <tr>
      <th>2013-01-07</th>
      <td>1006</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
    <tr>
      <th>2013-01-03</th>
      <td>1002</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
      <td>100</td>
      <td>B</td>
    </tr>
  </tbody>
</table>
</div>



五、类似分列的提取方式


```python
#1、观察数值组成结构，选择性提取
df_new['category_x'].str[:3]
```




    date
    2013-01-02    100
    2013-01-05    110
    2013-01-07    130
    2013-01-06    210
    2013-01-03    100
    2013-01-04    110
    Name: category_x, dtype: object



# 数据筛选


```python
# df_new.to_excel('pandas_test2.xls')
df_new = pd.read_excel('pandas_test2.xls')
df_new
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013-01-02</td>
      <td>1001</td>
      <td>BEIJING</td>
      <td>100-A</td>
      <td>23</td>
      <td>1200.0</td>
      <td>male</td>
      <td>10</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>100</td>
      <td>A</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-01-05</td>
      <td>1004</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>110</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013-01-07</td>
      <td>1006</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013-01-06</td>
      <td>1005</td>
      <td>SHANGHAI</td>
      <td>210-A</td>
      <td>34</td>
      <td>NaN</td>
      <td>male</td>
      <td>40</td>
      <td>N</td>
      <td>low</td>
      <td>NaN</td>
      <td>210</td>
      <td>A</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-01-03</td>
      <td>1002</td>
      <td>SH</td>
      <td>100-B</td>
      <td>44</td>
      <td>NaN</td>
      <td>female</td>
      <td>12</td>
      <td>N</td>
      <td>high</td>
      <td>NaN</td>
      <td>100</td>
      <td>B</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013-01-04</td>
      <td>1003</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
      <td>high</td>
      <td>1.0</td>
      <td>110</td>
      <td>A</td>
    </tr>
  </tbody>
</table>
</div>



按条件筛选（或与非）


```python
#1、与条件联合筛选
df_new[(df_new['age']>30) & (df_new['category_y']>200)][['city','age','category_y']]
df_new.loc[ (df_new['age']>30) & (df_new['category_y']>200),['city','age','category_y']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>age</th>
      <th>category_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>SHANGHAI</td>
      <td>34</td>
      <td>210</td>
    </tr>
  </tbody>
</table>
</div>




```python
#2、或条件联合筛选
df_new[(df_new['city']=='BEIJING')|(df_new['age']>40)][['city','age']]
df_new.loc[(df_new['city']=='BEIJING')|(df_new['age']>40),['city','age']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>BEIJING</td>
      <td>23</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BEIJING</td>
      <td>32</td>
    </tr>
    <tr>
      <th>4</th>
      <td>SH</td>
      <td>44</td>
    </tr>
    <tr>
      <th>5</th>
      <td>GUANGZHOU</td>
      <td>54</td>
    </tr>
  </tbody>
</table>
</div>




```python
#3、非条件筛选
df_new[df_new['city']!='BEIJING'][['city','age']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>SHENZHEN</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>SHANGHAI</td>
      <td>34</td>
    </tr>
    <tr>
      <th>4</th>
      <td>SH</td>
      <td>44</td>
    </tr>
    <tr>
      <th>5</th>
      <td>GUANGZHOU</td>
      <td>54</td>
    </tr>
  </tbody>
</table>
</div>




```python
#4、通用筛选函数，简化代码
df_new.query('age > 30 & city == ["BEIJING"]')#★★注意括号里的引号要用双引号
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>2013-01-07</td>
      <td>1006</td>
      <td>BEIJING</td>
      <td>130-F</td>
      <td>32</td>
      <td>4432.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>130</td>
      <td>F</td>
    </tr>
  </tbody>
</table>
</div>




```python
#5、类似sumif、countif功能
df_new[df_new['city']!='BEIJING'][['city','age']].age.sum()#求某列和
df_new[df_new['city']=='BEIJING'][['city','age']].city.count()#求某列技术
```




    2



# 数据汇总

分类汇总：ground by


```python
#1、指定字段或方式进行汇总,取字段，然后计数或求和
df_new.groupby('pay')['age'].count()
```




    pay
    N    2
    Y    4
    Name: age, dtype: int64



#2、多字段多级别分组进行汇总
df_new.groupby(['pay','size'])['age'].count()


```python
#3、多级别分组后，可多级运算
a = df_new.groupby(['pay','size'])['age']
a.agg([np.sum,np.mean,len])#agg函数，可添加多个函数
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>sum</th>
      <th>mean</th>
      <th>len</th>
    </tr>
    <tr>
      <th>pay</th>
      <th>size</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">N</th>
      <th>A</th>
      <td>34</td>
      <td>34.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>B</th>
      <td>44</td>
      <td>44.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th rowspan="3" valign="top">Y</th>
      <th>A</th>
      <td>77</td>
      <td>38.5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>C</th>
      <td>32</td>
      <td>32.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>F</th>
      <td>32</td>
      <td>32.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
数据透视 pivot_table
```


```python
#1、数据透视表，设置行、列、数据值、运算函数、空值填充
pd.pivot_table(df_new,index=['city'],columns=['size'],values='price',aggfunc=[len,np.mean,np.sum],fill_value=0)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">len</th>
      <th colspan="4" halign="left">mean</th>
      <th colspan="4" halign="left">sum</th>
    </tr>
    <tr>
      <th>size</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>F</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>F</th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>F</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>BEIJING</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1200</td>
      <td>0</td>
      <td>0</td>
      <td>4432</td>
      <td>1200</td>
      <td>0</td>
      <td>0</td>
      <td>4432</td>
    </tr>
    <tr>
      <th>GUANGZHOU</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2133</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2133</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>SH</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>SHANGHAI</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>SHENZHEN</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5433</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>5433</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



# 数据统计

数据抽样 sample


```python
#1、数据简单随机抽样
df_new.sample(2,replace=False)#重要参数是否放回，即重复或不重复抽样，默认不重复
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>id</th>
      <th>city</th>
      <th>category_x</th>
      <th>age</th>
      <th>price</th>
      <th>gender</th>
      <th>m-point</th>
      <th>pay</th>
      <th>group</th>
      <th>sign</th>
      <th>category_y</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>2013-01-04</td>
      <td>1003</td>
      <td>GUANGZHOU</td>
      <td>110-A</td>
      <td>54</td>
      <td>2133.0</td>
      <td>male</td>
      <td>20</td>
      <td>Y</td>
      <td>high</td>
      <td>1.0</td>
      <td>110</td>
      <td>A</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013-01-05</td>
      <td>1004</td>
      <td>SHENZHEN</td>
      <td>110-C</td>
      <td>32</td>
      <td>5433.0</td>
      <td>female</td>
      <td>40</td>
      <td>Y</td>
      <td>low</td>
      <td>NaN</td>
      <td>110</td>
      <td>C</td>
    </tr>
  </tbody>
</table>
</div>



描述统计 Describe


```python
#2、描述性统计
df_new.describe().round(2).T#统计后去二位小数，并转置
```

    E:\Anaconda3\lib\site-packages\numpy\lib\function_base.py:3834: RuntimeWarning: Invalid value encountered in percentile
      RuntimeWarning)





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>id</th>
      <td>6.0</td>
      <td>1003.50</td>
      <td>1.87</td>
      <td>1001.0</td>
      <td>1002.25</td>
      <td>1003.5</td>
      <td>1004.75</td>
      <td>1006.0</td>
    </tr>
    <tr>
      <th>age</th>
      <td>6.0</td>
      <td>36.50</td>
      <td>10.88</td>
      <td>23.0</td>
      <td>32.00</td>
      <td>33.0</td>
      <td>41.50</td>
      <td>54.0</td>
    </tr>
    <tr>
      <th>price</th>
      <td>4.0</td>
      <td>3299.50</td>
      <td>1966.64</td>
      <td>1200.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5433.0</td>
    </tr>
    <tr>
      <th>m-point</th>
      <td>6.0</td>
      <td>27.00</td>
      <td>14.63</td>
      <td>10.0</td>
      <td>14.00</td>
      <td>30.0</td>
      <td>40.00</td>
      <td>40.0</td>
    </tr>
    <tr>
      <th>sign</th>
      <td>1.0</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>category_y</th>
      <td>6.0</td>
      <td>126.67</td>
      <td>42.27</td>
      <td>100.0</td>
      <td>102.50</td>
      <td>110.0</td>
      <td>125.00</td>
      <td>210.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''平均数'''
df_new['age'].mean()
'''中位数'''
df_new['age'].median()
'''众数'''
df_new['age'].mode()
```


```python
#3、标准差
df_new['age'].std()
```




    10.876580344942981




```python
#4、协方差
df_new.cov()#整体间
df_new['age'].cov(df_new['price'])#两个字段间
```




    -2255.833333333333




```python
#5、相关分析系数
df_new.corr()#整体相关系数
df_new['age'].corr(df_new['m-point'])#两个字段间相关
```




    -0.19483296280358281

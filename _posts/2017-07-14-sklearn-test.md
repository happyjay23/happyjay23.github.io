---
layout: post
title: scikit-learn第二篇：关于sklearn实际建模中数据处理的流程
categories: scikit-learn
description: scikit-learn的数据处理和使用
keywords: sklearn, 数据预处理，特征选择，降维
---

![biaoti](/images/blog/2017-07-14_3.png)

# scikit-learn第二篇：关于sklearn实际建模中数据处理的流程 #

上一篇吧啦吧啦说了半天，实际干活又是怎么样的，就自己遇到的情况，总结了下面流程以实例演示

## sklearn数据处理特别注意几点：

1. sklearn的转换器（除了LabelEncoder和LabelBinarizer外）必须输入数值型数据，否则报错
2. sklearn的转换器（除了LabelEncoder和LabelBinarizer外）必须输入列结构数据，行结构数据（如pd.Series）需要使用reshape(-1,1)转为特征列后输入
3. sklearn的转换器（除了Imputer外）都不允许缺失值输入，否则报错
4. sklearn的转换器（除了OneHotEncoder外）都无法通过参数指定某个特征进行转化，所以要区别对待整体特征处理和局部特征处理
5. sklearn的转换器在`fit_transfrom`训练集后，测试集也必须采用训练集转换器进行`transfrom`


```python
import pandas as pd
import os
from sklearn.preprocessing import Imputer
from sklearn.preprocessing import Binarizer
from sklearn.preprocessing import LabelEncoder
from sklearn.preprocessing import OneHotEncoder
```


```python
os.chdir('C:/练习数据')
```


```python
'''设置1个案例数据'''
df = pd.read_excel('预处理案例.xlsx')
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>性别</th>
      <th>学历</th>
      <th>评分</th>
      <th>类别</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>男</td>
      <td>高中</td>
      <td>65.0</td>
      <td>class1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>男</td>
      <td>初中</td>
      <td>55.0</td>
      <td>class1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>女</td>
      <td>大学</td>
      <td>72.0</td>
      <td>class2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>NaN</td>
      <td>高中</td>
      <td>48.0</td>
      <td>class2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>女</td>
      <td>高中</td>
      <td>NaN</td>
      <td>class1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>女</td>
      <td>大学</td>
      <td>95.0</td>
      <td>class2</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''查看哪些列存在缺失值'''
df.isnull().sum()
```




    ID    0
    性别    1
    学历    0
    评分    1
    类别    0
    dtype: int64



## 数据预处理

### 一、部分特征数据和类别数据是定性数据，需处理为数值型数据


```python
'''方法一：利用map（字典）来映射'''
labellist = df['类别'].dropna().unique()
labeldict = {j:i for i,j in enumerate(labellist )}
df['类别'] = df['类别'].map(labeldict)

xlist = {j:i for i,j in enumerate(df['性别'].dropna().unique())}
df['性别'] = df['性别'].map(xlist)

x2list = {j:i for i,j in enumerate(df['学历'].unique())}
df['学历'] = df['学历'].map(x2list)
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>性别</th>
      <th>学历</th>
      <th>评分</th>
      <th>类别</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>65.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.0</td>
      <td>1</td>
      <td>55.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1.0</td>
      <td>2</td>
      <td>72.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>NaN</td>
      <td>0</td>
      <td>48.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1.0</td>
      <td>0</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>1.0</td>
      <td>2</td>
      <td>95.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''方法二：利用LabelEncoder编码'''
from sklearn.preprocessing import LabelEncoder
trf = LabelEncoder()
df['类别'] = trf.fit_transform(df['类别'])
df['学历'] = trf.fit_transform(df['学历'])
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>性别</th>
      <th>学历</th>
      <th>评分</th>
      <th>类别</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>男</td>
      <td>2</td>
      <td>65.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>男</td>
      <td>0</td>
      <td>55.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>女</td>
      <td>1</td>
      <td>72.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>NaN</td>
      <td>2</td>
      <td>48.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>女</td>
      <td>2</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>女</td>
      <td>1</td>
      <td>95.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''此方法对于存在缺失值时无法使用'''
df['性别'] = trf.fit_transform(df['性别'])
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    TypeError: unorderable types: str() > float()


### 二、处理缺失值问题


```python
'''删除缺失值：使用df.dropna()'''
df.dropna() #按行删除
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>性别</th>
      <th>学历</th>
      <th>评分</th>
      <th>类别</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>65.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.0</td>
      <td>1</td>
      <td>55.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1.0</td>
      <td>2</td>
      <td>72.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>1.0</td>
      <td>2</td>
      <td>95.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.dropna(axis=1) #按列删除
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>学历</th>
      <th>类别</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>2</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
'''填充缺失值：Imputer,默认采用均值填充'''
from sklearn.preprocessing import Imputer
'''情况1：整体使用Imputer，可能出逻辑错误，比如性别出现不男不女'''
trf = Imputer()
trf.fit_transform(df)
```




    array([[  1. ,   0. ,   0. ,  65. ,   0. ],
           [  2. ,   0. ,   1. ,  55. ,   0. ],
           [  3. ,   1. ,   2. ,  72. ,   1. ],
           [  4. ,   0.6,   0. ,  48. ,   1. ],
           [  5. ,   1. ,   0. ,  67. ,   0. ],
           [  6. ,   1. ,   2. ,  95. ,   1. ]])




```python
'''情况2：单独特征列的缺失值处理，可自行设置填充方式'''

'''离散型数据填充众数'''
trf = Imputer(strategy='most_frequent')
df['性别'] = trf.fit_transform(df['性别'].reshape(-1,1))
'''连续型数据调整均值'''
trf = Imputer()
df['评分'] = trf.fit_transform(df['评分'].reshape(-1,1))

df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>性别</th>
      <th>学历</th>
      <th>评分</th>
      <th>类别</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>65.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.0</td>
      <td>1</td>
      <td>55.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1.0</td>
      <td>2</td>
      <td>72.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1.0</td>
      <td>0</td>
      <td>48.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1.0</td>
      <td>0</td>
      <td>67.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>1.0</td>
      <td>2</td>
      <td>95.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### 三、定量数据二值化


```python
'''定量数据转为二值离散化:Binarizer'''
from sklearn.preprocessing import Binarizer
trf = Binarizer(threshold=60.0)
df['评分'] = trf.fit_transform(df['评分'].reshape(-1,1))
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>性别</th>
      <th>学历</th>
      <th>评分</th>
      <th>类别</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.0</td>
      <td>1</td>
      <td>0.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1.0</td>
      <td>2</td>
      <td>1.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1.0</td>
      <td>0</td>
      <td>0.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1.0</td>
      <td>0</td>
      <td>1.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>1.0</td>
      <td>2</td>
      <td>1.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



### 四、定性数据转为哑变量


```python
'''模型有时把名义变量可做顺序计算，此时需要转为哑变量：OneHotEncoder'''
from sklearn.preprocessing import OneHotEncoder
'''此方法可设置转换的数据列'''
trf = OneHotEncoder(categorical_features=[1],sparse=False)
trf.fit_transform(df)
```




    array([[ 1.,  0.,  1.,  0.,  1.,  0.],
           [ 1.,  0.,  2.,  1.,  0.,  0.],
           [ 0.,  1.,  3.,  2.,  1.,  1.],
           [ 0.,  1.,  4.,  0.,  0.,  1.],
           [ 0.,  1.,  5.,  0.,  1.,  0.],
           [ 0.,  1.,  6.,  2.,  1.,  1.]])



### 五、自定义函数转换


```python
'''当数据分布不均匀或者存在非线性关系使用：FunctionTransformer'''
from sklearn.preprocessing import FunctionTransformer
import numpy as np
'''参数func可设定转换函数'''
trf = FunctionTransformer(func=lambda x: x-0.5)
trf.fit_transform(df)
```




    array([[ 0.5, -0.5, -0.5,  0.5, -0.5],
           [ 1.5, -0.5,  0.5, -0.5, -0.5],
           [ 2.5,  0.5,  1.5,  0.5,  0.5],
           [ 3.5,  0.5, -0.5, -0.5,  0.5],
           [ 4.5,  0.5, -0.5,  0.5, -0.5],
           [ 5.5,  0.5,  1.5,  0.5,  0.5]])




```python
'''当存在非线性关系时使用：'''
from sklearn.preprocessing import PolynomialFeatures
'''参数degree可设定多项式次数'''
trf = PolynomialFeatures(degree=2)
trf.fit_transform(df)
```




    array([[  1.,   1.,   0.,   0.,   1.,   0.,   1.,   0.,   0.,   1.,   0.,
              0.,   0.,   0.,   0.,   0.,   0.,   0.,   1.,   0.,   0.],
           [  1.,   2.,   0.,   1.,   0.,   0.,   4.,   0.,   2.,   0.,   0.,
              0.,   0.,   0.,   0.,   1.,   0.,   0.,   0.,   0.,   0.],
           [  1.,   3.,   1.,   2.,   1.,   1.,   9.,   3.,   6.,   3.,   3.,
              1.,   2.,   1.,   1.,   4.,   2.,   2.,   1.,   1.,   1.],
           [  1.,   4.,   1.,   0.,   0.,   1.,  16.,   4.,   0.,   0.,   4.,
              1.,   0.,   0.,   1.,   0.,   0.,   0.,   0.,   0.,   1.],
           [  1.,   5.,   1.,   0.,   1.,   0.,  25.,   5.,   0.,   5.,   0.,
              1.,   0.,   1.,   0.,   0.,   0.,   0.,   1.,   0.,   0.],
           [  1.,   6.,   1.,   2.,   1.,   1.,  36.,   6.,  12.,   6.,   6.,
              1.,   2.,   1.,   1.,   4.,   2.,   2.,   1.,   1.,   1.]])



### 六、无量纲化


```python
from sklearn.preprocessing import StandardScaler
'''数据标准化至正态分布使用：StandardScaler'''
trf  = StandardScaler()
trf.fit_transform(df)
```




    array([[-1.46385011, -1.41421356, -0.92847669,  0.70710678, -1.        ],
           [-0.87831007, -1.41421356,  0.18569534, -1.41421356, -1.        ],
           [-0.29277002,  0.70710678,  1.29986737,  0.70710678,  1.        ],
           [ 0.29277002,  0.70710678, -0.92847669, -1.41421356,  1.        ],
           [ 0.87831007,  0.70710678, -0.92847669,  0.70710678, -1.        ],
           [ 1.46385011,  0.70710678,  1.29986737,  0.70710678,  1.        ]])




```python
from sklearn.preprocessing import MinMaxScaler
'''数据归一化缩放至[0,1]使用：MinMaxScaler()'''
trf = MinMaxScaler()
trf.fit_transform(df)
```




    array([[ 0. ,  0. ,  0. ,  1. ,  0. ],
           [ 0.2,  0. ,  0.5,  0. ,  0. ],
           [ 0.4,  1. ,  1. ,  1. ,  1. ],
           [ 0.6,  1. ,  0. ,  0. ,  1. ],
           [ 0.8,  1. ,  0. ,  1. ,  0. ],
           [ 1. ,  1. ,  1. ,  1. ,  1. ]])




```python
from sklearn.preprocessing import Normalizer
'''数据正则化至单位向量，以行计算时使用： Normalizer'''
trf = Normalizer()
trf.fit_transform(df)
```




    array([[ 0.70710678,  0.        ,  0.        ,  0.70710678,  0.        ],
           [ 0.89442719,  0.        ,  0.4472136 ,  0.        ,  0.        ],
           [ 0.75      ,  0.25      ,  0.5       ,  0.25      ,  0.25      ],
           [ 0.94280904,  0.23570226,  0.        ,  0.        ,  0.23570226],
           [ 0.96225045,  0.19245009,  0.        ,  0.19245009,  0.        ],
           [ 0.91499142,  0.15249857,  0.30499714,  0.15249857,  0.15249857]])



## 特征选择

### 1、Filter过滤


```python
'''方差选择法：VarianceThreshold'''
from sklearn.feature_selection import VarianceThreshold
'''参数threshold,来设定特征方差阈值筛选'''
trf = VarianceThreshold(threshold=0.5)
trf.fit_transform(df)
```




    array([[ 1.,  0.],
           [ 2.,  1.],
           [ 3.,  2.],
           [ 4.,  0.],
           [ 5.,  0.],
           [ 6.,  2.]])




```python
'''(有监督)检验系数选择法：SelectKBest'''
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import chi2
trf = SelectKBest(score_func=chi2,k='all')
x = df.iloc[:,1:-1]
y = df['类别']
trf.fit_transform(x,y)
```




    array([[ 0.,  0.,  1.],
           [ 0.,  1.,  0.],
           [ 1.,  2.,  1.],
           [ 1.,  0.,  0.],
           [ 1.,  0.,  1.],
           [ 1.,  2.,  1.]])



### 2、包装


```python
'''（有监督）递归特征消除法:RFE'''
from sklearn.feature_selection import RFE
from sklearn.linear_model import LogisticRegression
trf = RFE(estimator=LogisticRegression())
x = df.iloc[:,1:-1]
y = df['类别']
trf.fit_transform(x,y)
```




    array([[ 0.],
           [ 1.],
           [ 2.],
           [ 0.],
           [ 0.],
           [ 2.]])



### 3、嵌入


```python
'''（有监督）基于惩罚项的模型选择：'''
from sklearn.feature_selection import SelectFromModel
from sklearn.linear_model import LogisticRegression
trf = SelectFromModel(estimator=LogisticRegression())
x = df.iloc[:,1:-1]
y = df['类别']
trf.fit_transform(x,y)
```




    array([[ 0.,  0.],
           [ 0.,  1.],
           [ 1.,  2.],
           [ 1.,  0.],
           [ 1.,  0.],
           [ 1.,  2.]])




```python
'''（有监督）基于树的模型选择：'''
from sklearn.feature_selection import SelectFromModel
from sklearn.ensemble import GradientBoostingClassifier
trf = SelectFromModel(estimator=GradientBoostingClassifier())
x = df.iloc[:,1:-1]
y = df['类别']
trf.fit_transform(x,y)
```




    array([[ 0.],
           [ 0.],
           [ 1.],
           [ 1.],
           [ 1.],
           [ 1.]])



## 降维

### 1、无监督降维


```python
'''线性关系时，常用PCA主成分分析：PCA'''
from sklearn.decomposition import PCA
'''参数n_components设置主成分个数'''
trf = PCA(n_components=2)
trf.fit_transform(df)
```




    array([[-2.71746443, -0.2663753 ],
           [-1.65000369,  0.31511517],
           [-0.07262377,  1.36510453],
           [ 0.42406248, -0.84873696],
           [ 1.26797179, -1.22160493],
           [ 2.74805762,  0.65649748]])




```python
'''非线性关系时，核PCA主成分分析：KernelPCA'''
from sklearn.decomposition import KernelPCA
trf = KernelPCA(n_components=2)
trf.fit_transform(df)
```




    array([[-2.71746443, -0.2663753 ],
           [-1.65000369,  0.31511517],
           [-0.07262377,  1.36510453],
           [ 0.42406248, -0.84873696],
           [ 1.26797179, -1.22160493],
           [ 2.74805762,  0.65649748]])



### 2、有监督降维


```python
'''即降维又是模型，线性判别分析LDA：'''
from sklearn.lda import LDA
trf = LDA(n_components=2)
x = df.iloc[:,1:-1]
y = df['类别']
trf.fit_transform(x,y)
```




    array([[-2.83633617],
           [-0.80433414],
           [ 1.5663349 ],
           [ 0.80433414],
           [-0.29633363],
           [ 1.5663349 ]])




下一篇将研究下sklearn的流水线管道处理，通过流水线将数据处理和建模过程组合到一起

> ### 声明：未经本人同意，严禁转载，违者必究

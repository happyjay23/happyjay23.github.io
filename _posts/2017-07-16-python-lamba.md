---
layout: post
title: 关于python3中lambda和filter函数、map函数、reduce函数说明
categories: Python
description: python3中lambda和filter函数、map函数、reduce函数的用法
keywords: Python, lambda，filter，map,reduce,函数式编程
---

# 关于python3中lambda和filter函数、map函数、reduce函数 #

lambda作为一个表达式，定义了一个匿名函数，作用是简化了函数定义的书写形式，是代码更为简洁，更为直观，易理解

但不建议随意使用，可能发生莫名其妙的问题，比如sklearn中有些模块会报错！

### 注意事项：
在python3中，filter、map函数返回值均变为`迭代器`，需要通过list，tuple等处理后方能输出,而reduce输出值

## 一、lambda的定义形式


```python
'''例如下面例子，x作为函数输入，x+1作为函数输出'''
fun = lambda x:x+1
fun(1)
```




    2



## 二、filter函数，指定序列执行过滤操作


```python
'''例子：除法余数不为0的过滤'''
a = filter(lambda x:x%2 != 0,[1,2,3,4])
a
```




    <filter at 0x21fc828ffd0>




```python
list(a)
```




    [1, 3]



## 三、map函数，根据提供的函数对指定序列做映射


```python
'''例子：映射数据'''
a = map(lambda x:x+10,[1,2,3,4])
a
```




    <map at 0x21fc82932e8>




```python
list(a)
```




    [11, 12, 13, 14]



## 四、reduce函数，对参数序列中元素进行累积


```python
from functools import reduce
reduce(lambda x,y :x+y,[1,2,3,4])
```




    10


> ### 声明：未经本人同意，严禁转载，违者必究

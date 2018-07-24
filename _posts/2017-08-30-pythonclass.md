---
layout: post
title: 关于python3中staticmethod(静态方法)classmethod(类方法)实例方法的联系和区别
categories: Python
description: python3中staticmethod(静态方法)、classmethod(类方法)、实例方法
keywords: Python, class，静态方法，类方法,实例方法,私有变量
---

突然发觉自己好几天没写东西了，除了晚上加班，周末还得陪儿子好好玩耍，可能不小心又会荒废了自己的学习念头。

这阵子研究scrapy爬虫，涉及到了类的很多知识，其中关于类方法、静态方法、实例方法的区别和联系就是很有意思的一个方面~~

我从数据挖掘角度来看，python的类是数据集的特征属性集，而实例则是每个数据样本！

下面就这些内容，参考了网友和自己的学习体会，重新编写了一个案例，简单明了的案例

### 类变量
1、属于整个类，隐藏在类内部，对外不随意使用

2、用于整个类的内部调用，非某个方法专属


```python
class Expclass():
    cls_a = '类变量'
```

### 实例变量
1、属于类的实例化，对外提供使用

2、被实例对象调用或修改


```python
class Expclass():
    def __inif__(self):
        self.a = '实例变量'
```

### self和cls的概念和区别
1、self：在python中通常代表实例对象本身，作为定义实例方法的隐参数

2、cls：在python中通常代表类本身，用于定义类方法的隐参数

### classmethod的概念和特点
1、定义形式：@classmethod，将一个方法定义为类方法

2、可访问类变量，形式如cls.类变量，属于软编码

3、不能访问实例变量

### staticmethod的概念和特点
1、定义形式：@staticmethod，将一个方法定义为静态方法

2、可访问类变量，形式如类名.类变量，属于硬编码

3、不能访问实例变量


```python
'''定义个例子'''
class Expclass():

    #定义：类变量
    cls_a = '类变量'
    #定义：实例变量
    def __init__(self):
        self.a = '实例变量'
    #定义：类方法，使用隐参cls，代表类本身
    @classmethod
    def classdef(cls,text):
        c = text.split('-')
        print('--类方法--')
        print(c)
        print(cls.cls_a) #此处软编码调用类变量：cls.类变量
        print(cls.a)     #此处报错，类方法无法调用实例变量！！
    #定义：静态方法
    @staticmethod
    def staticdef(text):
        c = text.split('-')
        print('--静态方法--')
        print(c)
        print(Expclass.cls_a) #此处硬编码调用类变量：类名.类变量
        print(self.a)         #此处报错，静态方法无法调用实例变量！！
    #定义：实例方法，使用隐参self,代表实例本身
    def objectdef(self,text):
        c = text.split('-')
        print('--实例方法--')
        print(c)
        print(self.cls_a)     #此处实例调用类变量：self.类变量
        print(self.a)         #此处实例调用实例变量：self.实例变量
```


```python
'''实际测试：方法实例化'''
demo = Expclass()
```


```python
'''直接调用类方法'''
Expclass.classdef('测试-文本')
```

    --类方法--
    ['测试', '文本']
    类变量



    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-6-aac421d3fac5> in <module>()
          1 '''直接调用类方法'''
    ----> 2 Expclass.classdef('测试-文本')


    <ipython-input-1-865ab624379a> in classdef(cls, text)
         14         print(c)
         15         print(cls.cls_a) #此处软编码调用类变量：cls.类变量
    ---> 16         print(cls.a)     #此处报错，类方法无法调用实例变量！！
         17     #定义：静态方法
         18     @staticmethod


    AttributeError: type object 'Expclass' has no attribute 'a'



```python
'''实例化调用类方法'''
demo.classdef('测试-文本')
```

    --类方法--
    ['测试', '文本']
    类变量



    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-7-c129a55a8f9f> in <module>()
          1 '''实例化调用类方法'''
    ----> 2 demo.classdef('测试-文本')


    <ipython-input-1-865ab624379a> in classdef(cls, text)
         14         print(c)
         15         print(cls.cls_a) #此处软编码调用类变量：cls.类变量
    ---> 16         print(cls.a)     #此处报错，类方法无法调用实例变量！！
         17     #定义：静态方法
         18     @staticmethod


    AttributeError: type object 'Expclass' has no attribute 'a'



```python
'''直接调用静态方法'''
Expclass.staticdef('测试-文本')
```

    --静态方法--
    ['测试', '文本']
    类变量



    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-9-ce18ad6012cc> in <module>()
          1 '''直接调用静态方法'''
    ----> 2 Expclass.staticdef('测试-文本')


    <ipython-input-1-865ab624379a> in staticdef(text)
         22         print(c)
         23         print(Expclass.cls_a) #此处硬编码调用类变量：类名.类变量
    ---> 24         print(self.a)         #此处报错，静态方法无法调用实例变量！！
         25     #定义：实例方法，使用隐参self,代表实例本身
         26     def objectdef(self,text):


    NameError: name 'self' is not defined



```python
'''实例化调用静态方法'''
demo.staticdef('测试-文本')
```

    --静态方法--
    ['测试', '文本']
    类变量



    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-10-9106ff871328> in <module>()
          1 '''实例化调用静态方法'''
    ----> 2 demo.staticdef('测试-文本')


    <ipython-input-1-865ab624379a> in staticdef(text)
         22         print(c)
         23         print(Expclass.cls_a) #此处硬编码调用类变量：类名.类变量
    ---> 24         print(self.a)         #此处报错，静态方法无法调用实例变量！！
         25     #定义：实例方法，使用隐参self,代表实例本身
         26     def objectdef(self,text):


    NameError: name 'self' is not defined



```python
'''直接调用实例方法'''
Expclass.objectdef('测试-文本')
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-11-17fb96aec4c8> in <module>()
          1 '''直接调用实例方法'''
    ----> 2 Expclass.objectdef('测试-文本')


    TypeError: objectdef() missing 1 required positional argument: 'text'



```python
'''实例化调用实例方法'''
demo.objectdef('测试-文本')
```

    --实例方法--
    ['测试', '文本']
    类变量
    实例变量


### 总结
1、类方法和静态方法均可访问类变量，形式不同。都不能访问实例变量

2、实例方法也可访问类变量，在变量名称相同时，它存在优先选择顺序即：实例变量>类变量>父类变量

> ## 原创内容未经作者同意，严禁转载

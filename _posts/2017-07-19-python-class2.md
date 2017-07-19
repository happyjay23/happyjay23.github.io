---
layout: post
title: 关于python3 类变量和类属性的学习
categories: Python
description: python3中类属性和变量
keywords: Python, class，类，子类,变量,私有变量
---

# 关于python3 类变量和类属性的学习 #

总结几点：类变量和类属性有不同点和相同点

不同点：
1. 类属性定义：公有属性`self.属性`，私有属性`self.__属性`

   类变量定义：公有变量`属性`，私有属性`__属性`

2. 类属性改变：`实例.属性=`

   类变量改变：`类.变量=`

相同点：
1. 在类中调用类属性和类变量的调用格式相同：

   公有属性/变量`self.属性/变量`，私有属性`self.__属性/变量`

2. 在子类中调用类属性和类变量的格式相同：

   公有属性/变量`self.属性/变量`，私有属性`self._类__属性/变量`

### 定义类和子类


```python
#定义类
class One:
    #定义类变量
    a = 0
    b = 0
    #定义私有类变量
    __c = 10

    #定义类的构造函数
    def __init__(self,name,age):
        #定义类的属性
        self.name = name
        #定义类的私有属性
        self.__age = age
    #定义类的方法
    def infor(self):
        #内部调用类的公共属性和私有属性，格式同类的变量
        return '{0}:{1}'.format(self.name,self.__age)
    #定义类的方法
    def text(self):
        #内部调用类的公共变量和私有变量，格式同类的属性
        return self.a,self.b,self.__c


#定义子类    
class Two(One):
    #定义同名的变量，来覆盖父类的变量
    a = 1
    b = 1
    #定义同名的私有变量，来覆盖父类的私有变量
    _One__c = 4.5
    #定义子类独立的私有变量
    __c = 2
    #定义子类的构造函数，并继承父类的属性
    def __init__(self,name,age):
        super().__init__(name,age)
    #定义同名的子类方法，调用父类的属性。此时覆盖父类的方法
    def infor(self):
        return '姓名{0}:年龄{1}'.format(self.name,self._One__age)
    #定义同名的子类方法，同时调用父类的私有变量、子类的私有变量。此时覆盖父类的方法
    def text(self):
        return self.a,self.b,self._One__c,self.__c
```

### 子类能继承类的变量、类的私用变量，且能覆盖。同时可定义子类自己的变量和私有变量


```python
One.__dict__,Two.__dict__
```




    (mappingproxy({'_One__c': 10,
                   '__dict__': <attribute '__dict__' of 'One' objects>,
                   '__doc__': None,
                   '__init__': <function __main__.One.__init__>,
                   '__module__': '__main__',
                   '__weakref__': <attribute '__weakref__' of 'One' objects>,
                   'a': 0,
                   'b': 0,
                   'infor': <function __main__.One.infor>,
                   'text': <function __main__.One.text>}),
     mappingproxy({'_One__c': 4.5,
                   '_Two__c': 2,
                   '__doc__': None,
                   '__init__': <function __main__.Two.__init__>,
                   '__module__': '__main__',
                   'a': 1,
                   'b': 1,
                   'infor': <function __main__.Two.infor>,
                   'text': <function __main__.Two.text>}))



### 类和子类的实例化


```python
exp1 = One('tom',22)
exp2 = Two('tom',22)
```

### 类实例的属性


```python
exp1.__dict__
```




    {'_One__age': 22, 'name': 'tom'}



### 子类实例的属性，它能继承了父类的属性，包括私有属性


```python
exp2.__dict__
```




    {'_One__age': 22, 'name': 'tom'}



### 类实例的方法:输出类变量，类的私用变量


```python
exp1.text()
```




    (0, 0, 10)



### 子类实例的方法：输出子类继承并覆盖后的类变量、类私有变量、以及子类自己的私有变量


```python
exp2.text()
```




    (1, 1, 4.5, 2)



### 类实例的方法：输出调用类的属性和私有属性


```python
exp1.infor()
```




    'tom:22'



### 子类实例的方法：输出继承的类属性和私有属性，并覆盖过的类方法


```python
exp2.infor()
```




    '姓名tom:年龄22'



### 特别说明：类变量的值无法通过实例调用（实例.变量=）来改变，必须通过类的调用（类.变量=）来改变

### 1. 实例调用变量通过赋值，无法改变变量的，只是为实例增加了一个属性而已！


```python
exp.a = 2 #此时只是增加了实例的一个属性而已！
exp.__dict__
```




    {'_One__age': 22, 'a': 2, 'name': 'tom'}




```python
Two.__dict__ #此时类的变量并未改变！
```




    mappingproxy({'_One__c': 4.5,
                  '_Two__c': 2,
                  '__doc__': None,
                  '__init__': <function __main__.Two.__init__>,
                  '__module__': '__main__',
                  'a': 1,
                  'b': 1,
                  'infor': <function __main__.Two.infor>,
                  'text': <function __main__.Two.text>})



### 2. 改变类变量的值，只能通过类调用的方法


```python
Two.a = 2 #此时变量值才能改变
Two.__dict__
```




    mappingproxy({'_One__c': 4.5,
                  '_Two__c': 2,
                  '__doc__': None,
                  '__init__': <function __main__.Two.__init__>,
                  '__module__': '__main__',
                  'a': 2,
                  'b': 1,
                  'infor': <function __main__.Two.infor>,
                  'text': <function __main__.Two.text>})


>### 声明：未经作者同意，不得转载，违者必究

---
layout: post
title: 关于python3 类变量和类属性的学习
categories: Python
description: python3中类属性和变量
keywords: Python, class，类，子类,变量,私有变量
---
# 关于python3 类变量和类属性的学习 #

![lei](/images/blog/2017-07-19_0.png)

属性分为类属性和实例属性，且分为公有属性和私有属性

类属性属于类所有，定义方式：`公有属性=/__私有属性=`

实例属性属于实例所有，定义方式：`self.公有属性=/self.__私有属性=`

实例可调用全部属性，若名称相同则优先调用实例属性，其次是类属性、最后是父类属性

类调用自身属性和实例属性的格式：

内部调用格式相同：`self.公有属性/self.__私有属性`

外部调用格式相同：`实例.公有属性/实例._类__私有属性`

子类可继承类属性和实例属性，包括私有属性

子类调用继承的类属性和实例属性格式：

内部调用格式相同：`self.公有属性/self._类__私有属性`

外部调用格式相同：`实例.公有属性/实例._类__私有属性`

类属性的修改格式统一为：`类.公有属性/类._类__私有属性`

## 一、定义类和子类


```python
#定义类
class A:
    #定义类的公有属性
    a = 1
    #定义类的私有属性
    __b = 2  
    #定义类的构造函数
    def __init__(self):
        #定义实例的公有属性
        self.name = 1
        #定义实例的私有属性
        self.__age = 1
    #定义类的实例方法
    def test(self):
        #分别调用类的私有属性、实例的私有属性、类的公有属性、实例的公有属性
        return self.__b,self.__age,self.a,self.name
#定义子类
class B(A):
    #修改继承来的类的私有属性
    A._A__b = 4
    #定义子类的构造函数，引入实例属性
    def __init__(self):
        super().__init__()
    #定义子类的实例方法，覆盖了继承来的类的实例方法
    def test(self):
        #分别调用继承来的类的私有属性、实例的私有属性、类的公有属性、实例的公有属性，并加以改造
        return self._A__b,self._A__age+1,self.a+1,self.name+1
```

## 二、观察类属性和实例属性的继承、调用情况

子类的属性会继承类的属性，包括类私有属性,建议利用dir查看


```python
dir(A),dir(B)
```




    (['_A__b',
      'a',
      'test'],
     ['_A__b',
      'a',
      'test'])


而使用.__dict__只是查看当前类的属性，无法查看其继承属性


```python
A.__dict__,B.__dict__
```




    (mappingproxy({'_A__b': 4,
                   '__dict__': <attribute '__dict__' of 'A' objects>,
                   '__doc__': None,
                   '__init__': <function __main__.A.__init__>,
                   '__module__': '__main__',
                   '__weakref__': <attribute '__weakref__' of 'A' objects>,
                   'a': 1,
                   'test': <function __main__.A.test>}),
     mappingproxy({'__doc__': None,
                   '__init__': <function __main__.B.__init__>,
                   '__module__': '__main__',
                   'test': <function __main__.B.test>}))



子类继承了类的公有属性,证明可继承


```python
A.a,B.a
```




    (1, 1)



子类继承了类的私有属性，证明可继承且可通过子类内部修改类的私有属性后，子类和类的属性均同步变化


```python
A._A__b,B._A__b
```




    (4, 4)



子类的实例属性会继承类的实例属性，包括实例私有属性


```python
a = A()
b = B()
a.__dict__,b.__dict__
```




    ({'_A__age': 1, 'name': 1}, {'_A__age': 1, 'name': 1})



子类的实例方法会继承类的实例方法，且可覆盖。同时私有属性内部调用格式相同：self._类__属性


```python
a.test(),b.test()
```




    ((4, 1, 1, 1), (4, 2, 2, 2))



类属性的修改方式，注意不是 案例. 而是 类.


```python
A._A__b = 5
A.__dict__,B._A__b
```


    (mappingproxy({'_A__b': 5,
                   '__dict__': <attribute '__dict__' of 'A' objects>,
                   '__doc__': None,
                   '__init__': <function __main__.A.__init__>,
                   '__module__': '__main__',
                   '__weakref__': <attribute '__weakref__' of 'A' objects>,
                   'a': 1,
                   'test': <function __main__.A.test>}),
     5)

>### 声明：未经作者同意，严禁转载，违者必究

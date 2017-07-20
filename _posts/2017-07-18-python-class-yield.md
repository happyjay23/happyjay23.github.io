---
layout: post
title: 关于python3中类class、以及yield的学习
categories: Python
description: python3中类class、以及yield函数的用法
keywords: Python, class，yield，继承,封装,多态
---

# 关于python3中类class、以及yield的学习 #

## 类的简单说明：

- 类class，主要有三大特性：封装、继承、多态
- 类包括属性和方法，其中分为公有和私有
- 定义公有属性和方法的格式：`self.属性`和`def 方法`
- 定义私有属性和方法的格式：`self.__属性`和`def __方法`
- 实例可直接调用公共属性或公共函数
- 实例不可直接调用私有属性或私有方法
- 在类的内部调用私有属性和方法的格式：`self.__属性`
- 在子类的内部调用父类的私有属性和方法的格式：`self._类__属性`

## 数据封装：数据和函数封装成类的属性和方法


```python
'''定义类'''
class Person:
    #定义构造方法
    def __init__(self,name,age):
        #定义实例私有属性
        self.__name = name
        #定义实例公共属性
        self.age  = age
    #定义公共方法，并内部调用私用方法   
    def say(self):
        self.__say()
    #定义私有方法，并内部调用实例私有属性
    def __say(self):
        print('姓名：{0},年龄：{1}'.format(self.__name,self.age))
```


```python
'''类的实例化'''
tom = Person('TOM',23)
```


```python
'''实例调用公共属性'''
tom.age
```




    23




```python
'''实例调用公共方法'''
tom.say()
```

    姓名：TOM,年龄：23



```python
'''实例不得调用私有属性，否则报错'''
tom.__name
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-47-77bed22a85d8> in <module>()
          1 '''私有属性不得实例使用，否则报错'''
    ----> 2 tom.__name


    AttributeError: 'Person' object has no attribute '__name'



```python
'''实例不得调用私有方法，否则报错'''
tom.__say()
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-49-3cb1c030b53f> in <module>()
          1 '''私有方法不得实例使用，否则报错'''
    ----> 2 tom.__say()


    AttributeError: 'Person' object has no attribute '__say'


## 继承:子类可继承父类的属性和方法，且子类可自定义自己的属性和方法


```python
'''定义子类'''
class Man(Person):

    def __init__(self,name,age,edu):

        #调用父类的构造函数
        super().__init__(name,age) #方法一使用super可简化
        #Person.__init__(self,name,age) #方法二使用此法

        #定义子类实例独立的属性
        self.edu = edu

    #定义子类的方法
    def mansay(self):
        print('子类的方法')
    #重构父类的方法
    def say(self):
        print('姓名：{0},年龄：{1},学历：{2}'.format(self._Person__name,self.age,self.edu))
```


```python
'''子类实例化'''
joy = Man('JOY',33,'高中')
```


```python
'''实例调用公开属性'''
joy.age
```




    33




```python
'''实例调用子类的方法'''
joy.mansay()
```

    子类的方法



```python
'''实例调用重构父类的方法'''
joy.say()
```

    姓名：JOY,年龄：33,学历：高中


## 多态：实际上就是一个接口，即调用同一个函数，产生不同的结果


```python
'''多态的体现'''
def fun(demo):
    demo.say()
fun(tom)
fun(joy)
```

    姓名：TOM,年龄：23
    姓名：JOY,年龄：33,学历：高中


## 使用yield的函数，此函数变成一个生成器也就是迭代器


```python
def demo(n):
    for i in n:
        item = {'old':0,'new':0}
        item['old'] = i
        item['new'] = i+2
        yield item # 每执行一次到此就会暂停下，并输出函数值，然后继续往下执行
    print('完成') #当全部执行完毕后，才会执行后面的内容
```


```python
a = demo([1,2,3])
for i in a:
    print(i)
```

    {'old': 1, 'new': 3}
    {'old': 2, 'new': 4}
    {'old': 3, 'new': 5}
    完成

>### 声明：未经本人同意，严禁转载，违者必究

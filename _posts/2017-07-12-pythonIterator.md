---
layout: post
title: 特别篇：看破迷魂阵—python可迭代对象、迭代器、生成器、列表生成式的关联和区别
categories: Python
description: python的可迭代对象、迭代器、生成器、列表生成式
keywords: Python, 迭代器，生成器，列表生成式
---

# 看破迷魂阵—python可迭代对象、迭代器、生成器、列表生成式的关联和区别#

当初这几个概念把我搞的晕头转向了很久，真是傻傻分不清楚，后来看了很多大神的博文才大概知道些原理

趁着还没忘记的时候，好好梳理下。结合着大神们的思路和自己的想法来整理的，错误的地方还请指出来改正。

![iterator](/images/blog/2017-07-12_0.png)

# 一、容器（container）

如list、dict、set都是容器

# 二、可迭代对象（iterable）

通常叫法。这个读法可能误导人以为这是独立的一个对象，其实它是一个范围的统称，个人觉得应该叫`可以迭代的数据结构（容器）`

python的很多容器本身就是可迭代对象，比如`list、dict、set`等


```python
'''先举个例子，定义一个列表，本身就是可迭代对象'''
explist = [1,2,3,4,5]
type(explist)
```
    list


# 三、迭代器（iterator）

`迭代器对象`是`可迭代对象`通过一定方法构造出来的


```python
'''方法一：利用for in 循环来构造,实际过程就是一个迭代器'''
for i in explist:
    ...
```


```python
'''方法二：利用iter()函数来构造'''
iter(explist)
```

    <list_iterator at 0x2d5c6f0>



# 四、生成器（generator）

生成器本身就是迭代器，是迭代器的一种特例


```python
'''方法一：利用包含yield的函数来构造'''
def gen():
    for i in explist:
        yield i+1
gen()
```

    <generator object gen at 0x02DB29C0>


```python
'''方法二：利用类似列表生产式方式来构造'''
( i+1 for i in explist)
```

    <generator object <genexpr> at 0x02DB2870>


# 五、列表生成推导式（list comprehension）
列表生成式就是生成器表达式的特例，只不过它构造的是列表，而不是生成器对象


```python
[i+1 for i in explist]
```

    [2, 3, 4, 5, 6]

# 总结：
    1、python的很多内置对象都是可迭代对象，如list，set，dict
    2、迭代器由可迭代对象来构造
    3、生成器属于迭代器的一种特例，简化迭代过程
    4、列表生成式是生成器表达式的一种特例，构造list
    5、可迭代对象、迭代器、生成器都可以使用for循环迭代，而迭代器和生成
       器拥有next()函数

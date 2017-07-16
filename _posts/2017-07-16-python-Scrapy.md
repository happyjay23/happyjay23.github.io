---
layout: post
title: 关于利用Anaconda3安装Scrapy的正确姿势
categories: Python
description: 利用Anaconda3安装Scrapy，避免各种坑各种报错
keywords: Python, Anaconda3，Scrapy，安装
---

# 关于利用Anaconda3安装Scrapy的正确姿势 #

Scrapy这个玩意安装起来其实非常麻烦，而且有各种坑等待着你

经过测试使用pip的方式，安装会提示Microsoft Visual C++ 14.0 is required

所以还是老老实实地用Anaconda3来安装吧，世界才能和平！

## 第一步：win+R输入cmd回车后，输入conda install Scrapy

![step1](/images/blog/2017-07-16_0.png)

## 第二步：提示安装一堆包，只需选择Y即可完成安装
```
C:\Users\XXX用户>conda install Scrapy
Fetching package metadata ...........
Solving package specifications: .

Package plan for installation in environment D:\Anaconda3:

The following NEW packages will be INSTALLED:

    attrs:            15.2.0-py36_0
    automat:          0.5.0-py36_0
    constantly:       15.1.0-py36_0
    cssselect:        1.0.1-py36_0
    hyperlink:        17.1.1-py36_0
    incremental:      16.10.1-py36_0
    parsel:           1.2.0-py36_0
    pyasn1-modules:   0.0.8-py36_0
    pydispatcher:     2.0.5-py36_0
    queuelib:         1.4.2-py36_0
    scrapy:           1.3.3-py36_0
    service_identity: 17.0.0-py36_0
    twisted:          17.5.0-py36_0
    w3lib:            1.17.0-py36_0
    zope:             1.0-py36_0
    zope.interface:   4.4.2-py36_0

The following packages will be UPDATED:

    conda:            4.3.14-py36_1  --> 4.3.22-py36_0

Proceed ([y]/n)? y

attrs-15.2.0-p 100% |###############################| Time: 0:00:00  79.69 kB/s
constantly-15. 100% |###############################| Time: 0:00:00 410.96 kB/s
cssselect-1.0. 100% |###############################| Time: 0:00:00 251.93 kB/s
hyperlink-17.1 100% |###############################| Time: 0:00:00 315.36 kB/s
incremental-16 100% |###############################| Time: 0:00:00 269.79 kB/s
pydispatcher-2 100% |###############################| Time: 0:00:00 291.55 kB/s
queuelib-1.4.2 100% |###############################| Time: 0:00:00   1.18 MB/s
zope-1.0-py36_ 100% |###############################| Time: 0:00:00  92.02 kB/s
automat-0.5.0- 100% |###############################| Time: 0:00:00 142.62 kB/s
pyasn1-modules 100% |###############################| Time: 0:00:00 189.75 kB/s
w3lib-1.17.0-p 100% |###############################| Time: 0:00:00 325.04 kB/s
zope.interface 100% |###############################| Time: 0:00:01 148.78 kB/s
parsel-1.2.0-p 100% |###############################| Time: 0:00:01  14.44 kB/s
twisted-17.5.0 100% |###############################| Time: 0:00:24 184.71 kB/s
conda-4.3.22-p 100% |###############################| Time: 0:00:03 136.57 kB/s
service_identi 100% |###############################| Time: 0:00:00 199.79 kB/s
scrapy-1.3.3-p 100% |###############################| Time: 0:00:02 147.51 kB/s
```

## 第三步：测试是否安装成功

import没问题

![step2](/images/blog/2017-07-16_1.png)

cmd下没有报错

![step3](/images/blog/2017-07-16_2.png)

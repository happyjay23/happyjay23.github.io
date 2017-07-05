---
layout: post
title: 搭建篇：菜鸟利用GitHubDesktop + jekll 30分钟搭建博客
categories: Blog
description: 傻瓜式GitHub博客搭建
keywords: GitHub, jekll，博客
---

# 搭建篇：菜鸟利用GitHubDesktop + jekll30分钟搭建博客 #

想搭个博客做学习笔记，研究了半天才发现避免不了GitHub，
之前真没接触过，经过一晚上恶补github，在看了git的各种命令、
配置、安装、报错后 几乎抓狂！！最后摸索了个简单的法子~~

> 声明：本人是菜鸟，什么都只是会一点儿，所以我把重点放在更新内容上，而不是鼓捣CSS、配色什么的。这里的方法可能不是最好最方便的，却挺适合本人这样的新手，欢迎相互交流

## 工具准备： ##
> 1. [GitHub账户](https://github.com)
> 2. [GitHubDesktop客户端](https://desktop.github.com/)
>分享：[使用介绍地址](http://blog.csdn.net/yuxin1100/article/details/52801878)
> 3. [jekll模板](https://github.com/mzlogin/mzlogin.github.io)
> 分享：[免费模板地址](http://jekyllthemes.org/)


## 一、注册GitHub账户 ##
1. 注册步骤

  进入首页可以看见如下图

  ![gj1](images\blog\gj1.png)

  依次输入注册信息（用户名，邮箱，密码）点击注册，跳转至如下画面

  ![gj2](images\blog\gj2.png)

  默认为FREE选项，然后点击结束注册

  ![gj3](images\blog\gj3.png)

  网站会发一封确认邮件到你的注册邮箱 登陆邮箱进行确认

2. 建立项目

  点击+ 选择新建项目

  ![gj4](images\blog\gj4.png)

  然后输入名称，注意Repository name务必和你的ID一致，可避免一些问题

  ![gj5](images\blog\gj5.png)

  创建成功之后，进入了项目主页面。点击设置按钮

  ![gj6](images\blog\gj6.png)

  滚到下面设置风格模板，有自己域名可以设置，否则为空

  ![gj7](images\blog\gj7.png)

  选择模板，并发布

  ![gj8](images\blog\gj8.png)
>其中可能有些步骤没写，一般默认即可，自己搞定吧



## 二、安装GitHubDesktop客户端 ##
1. 安装很简单，自己搞定吧，反正我没有遇到困难PS:win10系统已更新
2. 同步项目到本地

  ![gj9](images\blog\gj9.png)

  ![gj10](images\blog\gj10.png)
3. 也可以从客户端下载项目到本地

  ![gj11](images\blog\gj11.png)
>客户端操作比较简单，前面提供了网友的介绍，可参考



## 三、下载并上传开源的Jekyll模板 ##
1. 下载模板有2种方法

  直接Fork网友的公开项目

  ![gj12](images\blog\gj12.png)

  也可以通过github打包下载

  ![gj13](images\blog\gj13.png)

  将自己项目下的只留隐藏文件夹git，其他删除，然后将下载文件复制到本地项目文件夹

  ![gj14](images\blog\gj14.png)

2. 利用客户端同步至GitHub

  填写后提交同步到github
  ![gj15](images\blog\gj15.png)

3. 修改模板文件参数

  . _config.yml：博客基本配置文件，里面有博客的名字，以及存放博主的一些基本信息

  这是最重要的文件，一般公开模板都是有说明的，可以参照修改

  .  _index.html：这是你博客的主页面，里面的内容就是你的主页了

  . _layouts：这文件夹里面存放你每个页面的设计，一般有default.html（默认页面）和posts.html（博文页面）

  . _includes：这个文件夹里的的内容将会通用到你博客每个页面，起到一种便利的作用

  . _posts：这里面装的就是你的博文啦，记住，要用markdown语法写，要不上传会失败的


完成了博客搭建，省下的就是自己编写markdown文件，放到_posts文件夹
然后同步到gibhut上就行了，简单吧！

至于站点样式、设计什么的，以后再慢慢研究
# 致谢多位大牛的文章以及[mazhuang](http://mazhuang.org/)的模板 #

> 下一篇将介绍下Markdown编辑器以及GitHub更新文章的操作

---
layout: page
title: About
description: 个人的思考
keywords: Data, 分析
comments: true
menu: 关于
permalink: /about/
---
![about](/pages/about.png)

## 本博客分享菜鸟的数据挖掘学习笔记 ##
## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## 应用技术

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}


  [1]: ./images/1499188954431.jpg

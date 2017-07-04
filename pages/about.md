---
layout: page
title: 关于
description: 个人的思考
keywords: Data, 分析
comments: true
menu: 关于
permalink: /about/
---
![enter description here][1]
本人是一个菜鸟数据爱好者，记录下平时学习经历和心得分享
## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}


  [1]: ./images/1499188954431.jpg
# 神烦DMer

个人博客：<https://happyjay23.github.io/>，本博客外观基于[码志的博客](http://mazhuang.org)修改

非常感谢，也欢迎Star和Fork

## 介绍
<!-- vim-markdown-toc GFM -->
* [Fork 说明](#fork-指南)
* [经验与思考](#经验与思考)
<!-- vim-markdown-toc -->

## Fork 说明

Fork本项目后，需要进行配置才能运行。

1. 正确设置项目名称与分支。

   按照 GitHub Pages 的规定，名称为 `username.github.io` 的项目的 master 分支，或者其它名称的项目的 gh-pages 分支可以自动生成 GitHub Pages 页面。

2. 修改域名。

   若绑定自己的域名，则修改CNAME文件的内容，否则删掉CNAME文件即可

3. 修改配置。

   网站的配置基本都集中在 \_config.yml 文件中，将其中与个人信息相关的部分替换成你自己的，比如网站的 title、subtitle 和 Disqus 的用户名等。
   ```
   # ---------------- #
   #   Main Configs   #
   # ---------------- #
   baseurl:          #为项目名，若项目是'***.github.io'，则设置为空''
   url: happyjay23.github.io    #个人网站地址
   date_format: "ordinal"      
   title: 神烦DMer              #博客名称
   subtitle: "数据即未来"        #博客题目
   description: "个人学习笔记"   #博客描述
   keywords: Data Mining, happyjay23, Data  #关键词
   favicon: /favicon.ico        #图标
   timezone: Asia/Shanghai      #时区
   encoding: "utf-8"
   side_bar_repo_limit: 5

   # ---------------- #
   #      Author      #
   # ---------------- #
   author: Hai Bo              # 作者名称
   organization: jay23.com     # 网站名称
   organization_url: https://happyjay23.github.io/  #网站地址
   github_username: happyjay23 # github ID
   location: HeBei, China      # 地区描述
   email: happyjay23@sina.com  # 个人邮箱

   # ---------------- #
   #      Jekyll      #
   # ---------------- #
   repository: happyjay23/happyjay23.github.io  #改成自己的

   # ---------------- #
   #      Comments    #
   # ---------------- #
   # support provider: disqus, netease_gentie, gitment
   comments_provider: gitment
   # !!!重要!!! 请修改下面这些信息为你自己申请的
   # !!!Important!!! Please modify infos below to yours
   # https://disqus.com
   disqus:
       username: mzlogin
   # https://gentie.163.com
   netease_gentie:
       productKey: 560ed243b708465d82d5a0a5f3b83da1
   # https://imsun.net/posts/gitment-introduction/
   gitment:
       owner: happyjay23     #自己的github ID
       repo: happyjay23.github.io   #自己的项目名，此处是大坑！
       oauth:                      #自己的OAuth
           client_id: 4d061788eeaabef903b8  
           client_secret: 8b323f24d7f51b0fa30a5396b43edfc369352619
   # 在使用其它评论组件时可点击显示 Disqus

   **注意：** 在注册OAuth的时候，Authorization callback URL务必是博客主页地址

   **注意：** 因为 Disqus 处理用户名与域名白名单的策略存在缺陷，请一定将 disqus.username 修改成你自己的。
   ```
  >![213](/images/blog/2017-07-08_1.png)
  >![213](/images/blog/2017-07-08_2.png)
  >![213](/images/blog/2017-07-08_3.png)

4. 删除我的文章与图片。

   如下文件夹中除了 template.md 文件外，都可以全部删除，然后添加你自己的内容。

   >* \_posts 文件夹中是我已发布的博客文章。
   >* \_drafts 文件夹中是我尚未发布的博客文章。
   >* \_wiki 文件夹中是我已发布的 wiki 页面。
   >* images 文件夹中是我的文章和页面里使用的图片。

5. 修改pages文件夹。

   >* pages/about.md 文件内容对应网站的「关于」页面，将它们替换成你自己的信息，包括 \_data 目录下的 skills.yml 和 social.yml
   >* pages/link.md 文件内容对应修改 \_data 目录下的 links.yml


## 经验与思考

* 简约，尽量每个页面都不展示多余的内容。


* 如果写技术文章，那先将技术原理完全理清了再开始写，一边摸索技术一边组织文章效率较低

* 杜绝难断句、难理解的长句子，如果不能将其拆分成几个简洁的短句，说明脑中的理解并不清晰。

* 可以学习一下那些高质量的博主，他们的行文，内容组织方式，有什么值得借鉴的地方。

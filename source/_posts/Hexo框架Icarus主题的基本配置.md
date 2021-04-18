---
title: Hexo框架Icarus主题的基本配置
toc: true
thumbnail: /gallery/thumbnails/Paris Street Rainy Day.jpg
widgets:
  - type: profile
    position: left
    author: 博雅
    author_title: zju小硕一枚
    location: 'Hangzhou,China'
    avatar: /images/Golden_Medal_-1_Icon.svg
    gravatar: null
    avatar_rounded: false
    follow_link: 'https://github.com/MasterXuBoya'
    social_links:
      Github:
        icon: fab fa-github
        url: 'https://github.com/MasterXuBoya'
      E-Mail:
        icon: fa fa-envelope
        url: xuboya@zju.edu.cn
      Facebook:
        icon: fab fa-facebook
        url: 'https://facebook.com'
      Twitter:
        icon: fab fa-twitter
        url: 'https://twitter.com'
      RSS:
        icon: fas fa-rss
        url: /
  - type: toc
    position: left
  - type: links
    position: left
    links:
      Hexo: 'https://hexo.io'
      振动台控制软件: 'https://zjudeveloper.gitbook.io/-2/'
  - type: category
    position: left
date: 2020-03-02 16:11:56
tags:
  - Hexo
  - Icarus
categories:
  - 个人网站
  - Hexo
---
# theme

## logo图标制作

[在线favicon制作工具](https://favicon.io/emoji-favicons/unicorn-face/)

## header

> header主要包括logo图标、菜单栏和小链接
<!--more-->
1. logo图标可以通过下边两个网址制作，一个logo图标通常由小图标+文字组成
[阿里巴巴矢量图工具](https://www.iconfont.cn/)
[在线svg制作logo工具](https://svg.wxeditor.com/)

2. 添加菜单栏目录的方法：

假如我们要添加一个叫"专题"的菜单栏，步骤如下：

在根目录下新建post，名称叫project
```bash
hexo new post project
```
此时自动在source目录下生成/project文件夹，里面有一个index.html，就是project菜单栏的默认显示内容。

在主题配置文件_config.yml中加入该菜单
```style _config.yml
navbar:
    # Navigation bar menu links
    menu:
        主页: /
+      专题: /project
        计划: /schedule
        时光轴: /archives
        分类: /categories
        标签: /tags
        关于我: /about
```

## sidebar

自定义侧边栏的方法：
+ 在`F:\hexo\blog\themes\icarus\layout\widget`目录下保存的就是默认的widget侧边栏，用户可以在此目录下新建ejs和js文件
+ 然后在主题配置文件_config.yml将该widget显示出来

```style _config.yml
widgets:
+   type: bulletin
+   position: right
```

## footer


## Markdown
[Code风格库](https://highlightjs.org/static/demo/)

# post
post配置的内容包括以下几个方面：
+ donate：Alipay、WeChatPay
+ comment：Valid
+ share：BaiduShare
+ analysis：busuanzi


## 参考博客
官网GitHub：<https://github.com/ppoffice/hexo-theme-icarus/issues?utf8=%E2%9C%93&q=%E4%BB%A3%E7%A0%81>
<https://dp2px.com/> 该作者的GitHub源码：<https://github.com/lxqxsyu/hexo-theme-icarus>
<https://removeif.github.io/> 该作者的GitHub源码：<https://github.com/removeif/hexo-theme-icarus-removeif>

---

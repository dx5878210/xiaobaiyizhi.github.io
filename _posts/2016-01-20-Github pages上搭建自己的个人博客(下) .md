---
layout:     post
title:      "Github pages 搭建个人blog(下)"
subtitle:   ""
date:       2016-01-20 00:00:00
author:     "小白一只"
header-img: "img/post-bg-02.jpg"
---


##基于github pages搭建个人blog（下）

####这次来介绍如何美化自己的blog
美化自己的blog其实有两张办法
1. 使用github pages提供的官方模板
1. 用户可以上传自定义模板

  上传的模板不是单纯的上传而是要经过一定的处理，要经过Jekyll的处理之后才能显示出来
[Jerkll](http://jekyllrb.com/ "Jerkll")是一个静态站点生成器，它会根据网页源码生成静态文件。它提供了模板、变量、插件等功能，所以实际上可以用来编写整个网站。 有兴趣可以参考Jerkll官网

####现在我们要采用第二种方法来美化blog
很多人可能会问那么复杂的网页会不会花很长时间来做，其实不会的，有一种简便的方法，就是使用别人开发好的模板
可以去[jekyllthemes](http://jekyllthemes.org/ "jekyllthemes")挑选自己的喜欢的模板然后下载下来解压的到自己的blog文件价里面。

#####例如这样:
![](https://raw.githubusercontent.com/xiaobaiyizhi/xiaobaiyizhi.github.io/master/img/create-firstblog/filefoler.png)

#####解压之后记得修改_config.yml里面的内容
```python
# Site settings
title: Xiaobai Blog
header-img: img/home-bg.jpg
email: username@gmail.com ##这里填上你的email
baseurl: ""
url: "http://xiaobaiyizhi.github.io" ##这里填上你的blog地址
github_username:  xiaobaiyizhi  
weibo_username: 2031623685

# Build settings
markdown: kramdown
highlighter: pygments
permalink: pretty
paginate: 5
exclude: ["less","node_modules","Gruntfile.js","package.json","README.md"]
```

然后将文件上传到你的github仓库上，然后刷新你的blog主页看到你的页面有没有变漂亮了

####不同的模板config.yml的内容可能不太一样

####对了通常blog的文章都是编辑成markdown格式存放在_post文件夹里面

####图片文件存放在img文件夹里面

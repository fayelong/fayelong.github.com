---
layout: post
title: "jekyllbootstrap中twitter主题中的code背景修改"
description: ""
category: jekyllbootstrap 
tags: [jekyllbootstrap]
---
{% include JB/setup %}

喜欢markdown的简单，不过jekyllboostrap中twitter主题默认code样式有背景色，感觉不太爽，怪怪的，就去把这个去掉了。

在username.github.com里查找
	
	background-color:#fee9cc;

有三个css文件，去把这个删除就好，在code样式下面的。

	code{background-color:#fee9cc;color:rgba(0, 0, 0, 0.75);padding:1px 3px;}

重新启动本地jekyll或者上传到github浏览效果。

还没什么时间去研究、修改这些样式，等把近期的事情忙完再去学习这些强大的工具。
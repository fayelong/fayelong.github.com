---
layout: post
title: "use jekyll bootstrap on github"
description: "jekyll on github page"
category: jekyll
tags: [jekyll]
---
{% include JB/setup %}

第一篇文档

这两天在弄github和jekyll,按照网上的资料自己弄了一个。没做多大修改就用了。现在对一些地方做一些记录。

基本上是按下面这个介绍来做的，虽然中间有点乱，但自己多试试也没多大问题了。

[http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html](http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html)

[http://hzmook.github.io/2012/07/01/use-jekyll-build-blog-on-github.html](http://hzmook.github.io/2012/07/01/use-jekyll-build-blog-on-github.html "http://hzmook.github.io/2012/07/01/use-jekyll-build-blog-on-github.html")

## 注意的地方： ##

- github本地需要一个ssh key，在github个人首页，setting，SSH Keys中可以添加。

- Windows平台出现中文乱码或如下信息：
	
		c:/Ruby192/lib/ruby/gems/1.9.1/gems/jekyll-0.11.0/lib/jekyll/convertible.rb:29:in read_yaml: invalid byte sequence in GBK (ArgumentError)


	需要将下面的文件

		c:/Ruby192/lib/ruby/gems/1.9.1/gems/jekyll-0.11.0/lib/jekyll/convertible.rb

	中的

		self.content = File.read(File.join(base, name))

	修改为

		self.content = File.read(File.join(base, name), :encoding => "utf-8")

- 注意_config.yml中的"key : value",冒号前后最好有空格..


- 无论是配置文件(如_config.yml),还是其它文件(如markdown)，编写是使用utf-8的无bom的编码格式。在notepad++中的格式可以选择。



- 使用git base时，用jekyll本地测试时，在username.github.com文件夹下，不要进入到其它文件夹

		jekyll server


	    $ git clone git@github.com:username/username.github.com.git //本地如果无远程代码，先做这步，不然就忽略
	    $ cd .ssh/username.github.com //定位到你blog的目录下
	    $ git pull origin master //先同步远程文件，后面的参数会自动连接你远程的文件
	    $ git status //查看本地自己修改了多少文件
	    $ git add . //添加远程不存在的git文件
	    $ git commit * -m "what I want told to someone"
	    $ git push origin master //更新到远程服务器上




---
layout: post
title: "freemarker转义"
description: "freemarker escape"
category: web
tags: [web]
---
{% include JB/setup %}

freemarker中的转义。#{showcontent.check}这些需要在jsp中保留的，在.ftl中可以这样标记：

	${r"#{showcontent.check}"}

注意，r后保留原始字符串，因此需要严格控制格式，如

	${r" #{showcontent.check}"}

与
	
	${r"#{showcontent.check}"}

是不同的。
---
layout: post
title: "freemarker的中文编码乱码问题"
description: "freemarker的中文编码乱码"
category: web
tags: [web]
---
{% include JB/setup %}

freemarker中文乱码

问题描述：有一个index.ftl文件，使用freemarker进行转换成index.jsp。原index.ftl是utf-8编码的，但生成index.jsp后用utf-8格式查看中文乱码。

网络上说的大多是在

cfg.setEncoding(Locale.CHINA, "UTF-8");

Template template = cfg.getTemplate("index.ftl","UTF-8");

但这些并没有解决问题。

其实不是freemarker的问题，是保存为index.jsp文件时，文件流

Writer out = new OutputStreamWriter(System.out);//这里将内容打印到控制台

默认使用GBK编码，因此保存出来的文件用utf-8打开是乱码。

下面是我的freemarker的部分关键代码。

{% highlight java %}

	// 在整个应用的生命周期中，这个工作应该只做一次
	// 创建和调整配置
	String output = "index.jsp";
	String path = getClass().getProtectionDomain().getCodeSource().getLocation().getPath();
	 if(path.indexOf("WEB-INF")>0){
         path = path.substring(0,path.indexOf("/WEB-INF/classes"));
     }else{
     }//java web的项目根目录
	System.out.println(path);

	Configuration cfg = new Configuration();
	cfg.setEncoding(Locale.CHINA, "UTF-8");
	cfg.setTemplateUpdateDelay(0); 
	cfg.setDirectoryForTemplateLoading(new File(path+"/template"));
	cfg.setObjectWrapper(new DefaultObjectWrapper());

	// 在整个应用的生命周期中，这个工作可以执行多次
	// 获取和创建模板
	Template template = cfg.getTemplate("index.ftl","UTF-8");
	// 创建数据模型
	Map<String, Object> root = new HashMap();

	-------------------中间省略号----------------------------------

	// Writer out = new OutputStreamWriter(System.out);//这里将内容打印到控制台
	//Writer out = new FileWriter(new File(path + "/index1.jsp"));
	File file = new File(path + "/" + output);
	Writer out = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(file),"UTF-8"));//关键部分
	template.process(root, out);
	out.flush();

{% endhighlight %}
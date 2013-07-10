---
layout: post
title: "Win7 安装Alfresco4.2b"
description: "Win7 安装Alfresco4.2"
category: Alfresco
tags: [Alfresco]
---
{% include JB/setup %}

项目基于Alfresco进行二次开发，现在把Alfresco的开发过程记录一下。

##运行环境：（32位）

JDK7，Tomcat7，Mysql5.5以上（这里我们均采用32的版本）

##安装步骤：

1.安装JDK7，配置环境变量

2.如果之前有配置Tomcat，把原Tomcat的环境变量删掉。没有的话，也不用安装，安装Alfresco过程会附带。

3.安装Mysql5.5，启动，并在数据库中创建一个名为alfresco的数据库。

4.前面3步完成后，准备开始安装。

5.双击alfresco安装包，出现安装界面，选择中文。

![](/images/Alfresco4-2-b/image001.png)
 
6.前进。

![](/images/Alfresco4-2-b/image002.png)
 
7.选择高级。

![](/images/Alfresco4-2-b/image003.png)
 
8.选择Java，其余的点掉。数据库使用我们自己的Mysql数据库，在后面配置。

![](/images/Alfresco4-2-b/image004.png)
 
9.选择安装文件夹

![](/images/Alfresco4-2-b/image005.png)
 
10.配置数据库相关属性。

![](/images/Alfresco4-2-b/image006.png)
 
	注意，由于使用自己的数据库，因此这里要填写正确的配置。
	JDBC URL：jdbc:mysql://localhost:3306/alfresco
	JDBC 驱动程序：com.mysql.jdbc.Driver
	数据库名称：alfresco
	用户名：root	（填写自己的用户名）
	密码：（填写自己的密码）
	确认：（填写自己的密码）

11.配置Tomcat端口。如果8080被其它程序占用则更改到可用的端口。

![](/images/Alfresco4-2-b/image007.png)
 
12.FTP端口，使用默认。

![](/images/Alfresco4-2-b/image008.png)
 
13.RMI端口，使用默认。

![](/images/Alfresco4-2-b/image009.png)
 
14.填写管理密码。

![](/images/Alfresco4-2-b/image010.png)
 
15.服务启动配置。选择手动。

![](/images/Alfresco4-2-b/image011.png)
 
16.启动安装。

![](/images/Alfresco4-2-b/image012.png)
 
17.完成安装后，把Mysql驱动包拷贝到Afresco的tomcat的lib目录下。这里使用

	mysql-connector-java-5.1.13-bin.jar。

18.启动Afresco服务，5分钟后，打开share的网站路径，可以看到成功。

![](/images/Alfresco4-2-b/image014.jpg)
 
19.输入用户名：admin，密码为安装过程中的密码，即可进入。

![](/images/Alfresco4-2-b/image016.jpg)
 
20.如果登录后出现用户认证错误等信息，应该是数据库连接不上。确保数据库服务开启，alfresco数据库的存在，查看修改数据库连接驱动配置，或者卸载后重新安装。

21.如果还是打开失败，可能是数据库连接出错。
 尝试使用新的数据库连接驱动：
打开

	\%Alfresco_Home%\tomcat\shared\classes

下的

	alfresco-global.properties

修改数据库驱动连接语句：

	db.driver=org.gjt.mm.mysql.Driver

再次重新启动alfresco。

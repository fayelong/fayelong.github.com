---
layout: post
title: "Win7下github connection timed out解决方案"
description: ""
category: git
tags: [git]
---
{% include JB/setup %}

晚上使用git push时提示
	
	ssh -T git@github.com
	ssh: connect to host github.com port 22: Connection timed out

因github的SSH默认端口22被屏蔽，这时，可以使用443端口访问github

这里显示Win7下的解决方案：在~/.ssh 目录中添加config配置文件

	feilong@FEILONG-PC ~
	$ cd ~/.ssh/
	
	feilong@FEILONG-PC ~/.ssh
	$ ls
	id_rsa  id_rsa.pub  known_hosts

	vi config

	Host github.com
	User ****@gmail.com
	Port 443
	Hostname ssh.github.com
	identityfile ~/.ssh/id_rsa

再次测试：

	ssh -T git@github.com
	



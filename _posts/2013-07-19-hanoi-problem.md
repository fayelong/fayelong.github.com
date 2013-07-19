---
layout: post
title: "汉诺塔问题的递归解法"
description: "hanoi problem"
category: algorithm
tags: [algorithm]
---
{% include JB/setup %}


汉诺塔：汉诺塔（又称河内塔）问题是源于印度一个古老传说的益智玩具。大梵天创造世界的时候做了三根金刚石柱子，在一根柱子上从下往上按照大小顺序摞着64片黄金圆盘。大梵天命令婆罗门把圆盘从下面开始按大小顺序重新摆放在另一根柱子上。并且规定，在小圆盘上不能放大圆盘，在三根柱子之间一次只能移动一个圆盘。

使用递归解决汉诺塔问题



> **使用递归的一个关键就是，我们先定义一个函数，不用急着去实现它， 但要明确它的功能。**

定义一个函数：

	void hanoi(int n, char src , char bri , char dst);
	//功能：把n个圆盘从src柱子移动到dst柱子，其中可以借助柱子bri

整个递归的过程：

	//1~n的圆盘在src上，bri和dst闲置
	1.初始状态：(1~n,0,0);
	
	//将1~n-1个圆盘从src移动到bri,可以借助dst作桥梁
	2.中间过程：(n,1~n-1,0);
	对应函数：hanoi(n-1,src,dst,bri);
	
	//然后将n从src移动到dst
	3.cout<<"Move disk "<<n<<" from "<<src<<" to "<<dst<<endl;
	
	//接着将1~n-1个圆盘从bri移动到dst，可以借助src作桥梁
	4.最终状态：(0,0,1~n);
	对应函数：hanoi(n-1,bri,src,dst);

	5.递归终止条件：n==1的时候
	对应代码：
	cout<<"Move disk "<<n<<" from "<<src<<" to "<<dst<<endl;

因此，整个汉诺塔的完整代码如下：

	#include <iostream>
	using namespace std;
	
	void hanoi(int n,char src,char bri,char dst)
	{
		if (n==1)
		{
			cout<<"Move disk "<<n<<" from "<< src <<" to "<< dst <<endl;
		}else
		{
			hanoi(n-1,src,dst,bri);
			cout<<"Move disk "<< n << " from "<< src << " to "<<dst<<endl;
			hanoi(n-1,bri,src,dst);
		}
	}
	
	int main()
	{
		int n = 7;
		hanoi(n,'A','B','C');
		return 0;
	}

参考资料：

- [http://hawstein.com/posts/3.4.html](http://hawstein.com/posts/3.4.html)


	
	
	



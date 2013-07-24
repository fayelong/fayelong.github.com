---
layout: post
title: "八皇后问题"
description: "八皇后问题"
category: algorithm
tags: [algorithm]
---
{% include JB/setup %}

8皇后是个经典的问题，如果使用暴力法，每个格子都去考虑放皇后与否，一共有264 种可能。所以暴力法并不是个好办法。由于皇后们是不能放在同一行的， 所以我们可以去掉“行”这个因素，即我第1次考虑把皇后放在第1行的某个位置， 第2次放的时候就不用去放在第一行了，因为这样放皇后间是可以互相攻击的。 第2次我就考虑把皇后放在第2行的某个位置，第3次我考虑把皇后放在第3行的某个位置， 这样依次去递归。每计算1行，递归一次，每次递归里面考虑8列， 即对每一行皇后有8个可能的位置可以放。找到一个与前面行的皇后都不会互相攻击的位置， 然后再递归进入下一行。找到一组可行解即可输出，然后程序回溯去找下一组可靠解。

我们用一个一维数组来表示相应行对应的列，比如c[i]=j表示， 第i行的皇后放在第j列。如果当前行是r，皇后放在哪一列呢？c[r]列。 一共有8列，所以我们要让c[r]依次取第0列，第1列，第2列……一直到第7列， 每取一次我们就去考虑，皇后放的位置会不会和前面已经放了的皇后有冲突。 怎样是有冲突呢？同行，同列，对角线。由于已经不会同行了，所以不用考虑这一点。 同列：c[r]==c[j]; 同对角线有两种可能，即主对角线方向和副对角线方向。 主对角线方向满足，行之差等于列之差：r-j==c[r]-c[j]; 副对角线方向满足， 行之差等于列之差的相反数：r-j==c[j]-c[r]。 只有满足了当前皇后和前面所有的皇后都不会互相攻击的时候，才能进入下一级递归。

{% highlight cpp %}

	#include<iostream>
	using namespace std;
	
	int c[20];
	int n=8;
	int count=0;
	void Print(int *p)
	{
		for (int i=0; i<n; ++i)
		{
			for (int j=0; j<n; j++)
			{
				if (p[i]==j)
				{
					cout<<"1 ";
				}
				cout<<"0 ";
			}
			cout<<endl;
		}
		cout<<endl;
	}
	
	void Search(int r)
	{
		if (r==n)
		{
			Print(c);
			count++;
			return;
		}
		for (int i=0; i<n; ++i)
		{
			c[r] = i;
			int ok=1;
			for (int j=0; j<r; j++)
			{
				if ((c[r]-c[j]==0)||(c[r]-c[j]==r-j)||(c[j]-c[r]==r-j))
				{
					ok = 0;
					break;
				}
			}
			if (ok)
			{
				Search(r+1);
			}
		}
	}
	
	int main()
	{
		Search(0);
		cout<<"total result: "<<count<<endl;
		return 0;
	}

{% endhighlight %}

参考资料：

- 作者：Hawstein 出处：[http://hawstein.com/posts/8.8.html](http://hawstein.com/posts/8.8.html)
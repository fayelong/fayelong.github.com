---
layout: post
title: "实现一个算法打印出n对括号的有效组合"
description: "实现一个算法打印出n对括号的有效组合"
category: algorithm 
tags: [algorithm]
---
{% include JB/setup %}

对于括号的组合，要考虑其有效性。比如说，)(， 它虽然也是由一个左括号和一个右括号组成，但它就不是一个有效的括号组合。 那么，怎样的组合是有效的呢？对于一个左括号，在它右边一定要有一个右括号与之配对， 这样的才能是有效的。所以，对于一个输出，比如说(()())， 从左边起，取到任意的某个位置得到的串，左括号数量一定是大于或等于右括号的数量， 只有在这种情况下，这组输出才是有效的。我们分别记左，右括号的数量为left和right， 如下分析可看出，(()())是个有效的括号组合。

	(, left = 1, right = 0
	((, left = 2, right = 0
	((), left = 2, right = 1
	(()(, left = 3, right = 1
	(()(), left = 3, right = 2
	(()()), left = 3, right = 3

这样一来，在程序中，只要还有左括号，我们就加入输出串，然后递归调用。 当退出递归时，如果剩余的右括号数量比剩余的左括号数量多，我们就将右括号加入输出串。 直到最后剩余的左括号和右括号都为0时，即可打印一个输出串。代码如下：

	{% highlight cpp %}

	#include <iostream>
	using namespace std;
	
	void PrintPair(int left, int right, char *str, int cnt)
	{
		if (left<0||right<0)
		{
			return;
		}
		if (left == 0 && right ==0)
		{
			for (int i = 0; i<cnt;++i)
			{
				cout<<str[i];
			}
			cout<<" "<<endl;
		}else
		{
			if (left>0)
			{
				str[cnt] = '(';
				PrintPair(left-1,right,str,cnt+1);
			}
			if (right>left)
			{
				str[cnt] = ')';
				PrintPair(left,right-1,str,cnt+1);
			}
		}
	}
	
	int main()
	{
		const int cnt = 3;
		char s[2*cnt];
		PrintPair(3,3,s,0);
		
		return 0;
	}

	{% endhighlight %}

参考资料：

- 作者：Hawstein 出处：[http://hawstein.com/posts/8.5.html](http://hawstein.com/posts/8.5.html "http://hawstein.com/posts/8.5.html")
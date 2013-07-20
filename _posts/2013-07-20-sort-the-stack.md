---
layout: post
title: "对栈排序"
description: " 写程序将一个栈按升序排序。对这个栈是如何实现的，你不应该做任何特殊的假设。程序中能用到的栈操作有：push | pop | peek | isEmpty"
category: algorithm
tags: [algorithm]
---
{% include JB/setup %}

 写程序将一个栈按升序排序。对这个栈是如何实现的，你不应该做任何特殊的假设。程序中能用到的栈操作有：push | pop | peek | isEmpty。   

    #include <iostream>
    #include <queue>
    #include <stack>
    using namespace std;

方案1，使用一个附加栈。将栈1出栈，如果栈2为空，则直接将数据压栈。如果栈2不为空，则栈2顶的数据相比较：如果栈2顶的数据比该值小，则直接压栈；否则，栈2出栈压回栈1，再比较，直到栈2为空或者栈2顶数据比该值小。

    stack<int> SortStack1(stack<int> s)
    {
    	stack<int> t;//临时栈
    	while (!s.empty())
    	{
    		int val = s.top();
    		s.pop();
    		while (!t.empty()&&t.top()>val)
    		{
    			s.push(t.top());
    			t.pop();
    		}
    		t.push(val);
    	}
    	return t;
    }

方案2：使用一个优先队列，原栈中的元素不断出栈然后插入优先队列， 直到原栈为空。然后再将优先队列中的元素不断压回原栈，这样操作后， 栈中的元素便有序化了。

    stack<int> SortStack2(stack<int> s)
    {
    	priority_queue<int,vector<int>,greater<int>> q;
    	while (!s.empty())
    	{
    		q.push(s.top());
    		s.pop();
    	}
    	while (!q.empty())
    	{
    		s.push(q.top());
    		q.pop();
    	}
    	return s;
    }

主函数：

    void PrintStack(stack<int> s)
    {
    	while (!s.empty())
    	{
    		cout<<s.top()<<" ";
    		s.pop();
    	}
    }

    int main(){
    	stack<int> s;
    	for (int i = 20;i>0;--i)
    	{
    		s.push(i);
    	}
    
    	stack<int> t = SortStack1(s);
    	PrintStack(t);
    
    	cout<<endl;
    
    	t = SortStack2(s);
    	PrintStack(t);
    
    	return 0;
    }

参考资料：

- 作者：Hawstein 出处：[http://hawstein.com/posts/3.6.html](http://hawstein.com/posts/3.6.html)
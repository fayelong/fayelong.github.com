---
layout: post
title: "使用两个栈实现一个队列"
description: "使用两个栈实现一个队列"
category: algorithm
tags: [algorithm]
---
{% include JB/setup %}

使用两个栈实现一个队列
    
    #include <iostream>
    #include <stack>
    using namespace std;

方法1,使用两个栈sin,sout，入队列时，将sout压入sin,然后数据压入栈sin，此时，sout是空的；出队列时，将sin压入sout，然后sout弹出，此时，sin是空的；

    template <typename T>
    class MyQueue1{
    public:
    	MyQueue1(){}
    	~MyQueue1(){}
    	void push(T val){
    		move(sout,sin);
    		sin.push(val);
    	}
    	void pop(){
    		move(sin,sout);
    		sout.pop();
    	}
    
    	T front(){
    		move(sin,sout);
    		return sout.top();
    	}
    
    	T back(){
    		move(sout,sin);
    		return sin.top();
    	}
    
    	int size(){
    		return sin.size()+sout.size();
    	}
    
    	bool empty(){
    		return sin.empty()&&sout.empty();
    	}
    
    	void move(stack<T> &src, stack<T> &dst){
    		while (!src.empty())
    		{
    			dst.push(src.top());
    			src.pop();
    		}
    	}
    private:
    	stack<T> sin,sout;
    
    };

方法2：方法1会导致大量的数据移动，无论是进队列还是出队列。改进如下：

入队列时，直接将数据压入sin，出栈时，如果sout不为空，则sout弹出，否则，将sin压入sout后，再弹出

    template <typename T>
    class MyQueue2{
    public:
    	MyQueue2(){}
    	~MyQueue2(){}
    
    	void push(T val){
    		sin.push(val);
    	}
    
    	void pop(){
    		if (!sout.empty())
    		{
    			sout.pop();
    		}else{
    			move(sin,sout);
    			sout.pop();
    		}
    	}
    
    	T front(){
    		move(sin,sout);
    		return sout.top();
    	}
    
    	T back(){
    		move(sout,sin);
    		return sin.top();
    	}
    
    	int size(){
    		return sin.size()+sout.size();
    	}
    
    	bool empty(){
    		return sin.empty()&&sout.empty();
    	}
    
    	void move(stack<T> &src,stack<T> &dst){
    		while (dst.empty())
    		{
    			while (!src.empty())
    			{
    				dst.push(src.top());
    				src.pop();
    			}
    		}
    	}
    
    private:
    	stack<T> sin, sout;
    };

主函数：

    int main(){
    	MyQueue1<int> mq1;
    	mq1.push(1);
    	mq1.push(2);
    	cout<<"the front value is:"<<mq1.front()<<endl;
    	cout<<"the back value is:"<<mq1.back()<<endl;
    
    	MyQueue2<int> mq2;
    	mq2.push(1);
    	mq2.push(2);
    	cout<<"the front value is:"<<mq2.front()<<endl;
    	cout<<"the back value is:"<<mq2.back()<<endl;
    
    	return 0;
    }

参考资料：

- 作者：Hawstein 出处：[http://hawstein.com/posts/3.5.html](http://hawstein.com/posts/3.5.html)
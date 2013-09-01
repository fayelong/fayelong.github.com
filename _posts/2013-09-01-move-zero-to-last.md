---
layout: post
title: "把0移至后面，非0整数移至前面并保持有序"
description: ""
category: 
tags: [algorithm]
---
{% include JB/setup %}

	#include <iostream>
	using namespace std;
	
	//使A把0移至后面，非0整数移至数组前面并保持有序，返回值为原数据中第一个元素为0的下标
	
	int Func(int *A,int nSize)
	{
		int flag=0;//判断出现的是否第一个0
		int index;//此时数组的下标
		int t = 1;
		for (int i = 0; i<nSize-1;++i)
		{
			if (flag==0&&A[i]==0)
			{
				flag = 1;
				index = i;
			}
	
			if (A[i]==0&&A[i+1]!=0)
			{
				A[i+1-t] = A[i+1];
				A[i+1] = 0;
			}else if (A[i]==0&&A[i+1]==0)
			{
				t++;//记录0的个数
			}
		}
		return index;
	}
	
	int main()
	{
		int a[18]={1,2,3,4,0,5,7,0,7,5,3,2,0,234,3,0,2,4};
		cout<<Func(a,18);
		for (int i=0;i<18;++i)
		{
			cout<<a[i]<<" ";
		}
		return 0;
	}
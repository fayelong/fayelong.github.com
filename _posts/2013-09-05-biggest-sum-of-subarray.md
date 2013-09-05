---
layout: post
title: "最大子数组和"
description: ""
category: algorithm
tags: [algorithm]
---
{% include JB/setup %}

输入一个整形数组，数组里有正数也有负数。 数组中连续的一个或多个整数组成一个子数组，每个子数组都有一个和。 求所有子数组的和的最大值。要求时间复杂度为O(n)。

	int maxSum(int *a,int n)
	{
		int max = a[0];
		int sum = 0;
		for(int j=0;j<n;++j)
		{
			if(sum>=0)//如果加上一个后，sum大于0，就加
				sum += a[j];
			else
				sum = a[j];//否则就不加，重置为当前的值
			if(max>sum)
				max = sum;
		}
		return max;
	}
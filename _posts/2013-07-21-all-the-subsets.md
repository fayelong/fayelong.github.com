---
layout: post
title: "返回一个集合中的所有子集"
description: "返回一个集合中的所有子集"
category: algorithm
tags: [algorithm]
---
{% include JB/setup %}


写一个函数返回一个集合中的所有子集。

add

对于一个集合，它的子集一共有2n 个(包括空集和它本身)。它的任何一个子集， 我们都可以理解为这个集合本身的每个元素是否出现而形成的一个序列。比如说， 对于集合{1, 2, 3}，空集表示一个元素都没出现，对应{0, 0, 0}； 子集{1, 3}，表示元素2没出现(用0表示)，1，3出现了(用1表示)，所以它对应 {1, 0, 1}。这样一来，我们发现，{1, 2, 3}的所有子集可以用二进制数000到111 的8个数来指示。泛化一下，如果一个集合有n个元素，那么它可以用0到2n -1 总共2n 个数的二进制形式来指示。每次我们只需要检查某个二进制数的哪一位为1， 就把对应的元素加入到这个子集就OK。

{% highlight c %}

	typedef vector<vector<int>> vvi;
	typedef vector<int> vi;
	vvi GetSubsets1(int a[], int n)
	{
		vvi subsets;
		int max = 1<<n;//1向左移n位，即2的n次方
		for (int i = 0; i < max; ++i)//从0到2^n-1遍历
		{
			vi subset;
			int j = i;
			int index = 0;
			while (j>0)
			{
				if (j&1)//判断j的最后一位是否为1，是的话将该位对应的存进向量
				{
					subset.push_back(a[index]);
				}
				j = j>>1;//j向右移动一位，然后继续循环判断最后一位是否为0
				++index;
			}
			subsets.push_back(subset);
		}
		return subsets;
	}
{% endhighlight %}


另一个思路是递归。这道题目为什么可以用递归？ 因为我们能找到比原问题规模小却同质的问题。比如我要求{1, 2, 3}的所有子集， 我把元素1拿出来，然后去求{2, 3}的所有子集，{2, 3}的子集同时也是{1, 2, 3} 的子集，然后我们把{2, 3}的所有子集都加上元素1后，又得到同样数量的子集， 它们也是{1, 2, 3}的子集。这样一来，我们就可以通过求{2, 3}的所有子集来求 {1, 2, 3}的所有子集了。而同理，{2, 3}也可以如法炮制。

{% highlight c %}
    vvi GetSubsets2(int a[], int index, int n)
    {
    	vvi subsets;
    	if (index == n)
    	{
    		vi subset;
    		subsets.push_back(subset);//递归终止条件，空集
    	}else
    	{
    		vvi rsubsets = GetSubsets2(a,index+1,n);//获取递归回来的集合
    		int v = a[index];
    		for (int i=0; i<rsubsets.size(); ++i)
    		{
    			vi subset = rsubsets[i];//获取递归回来的子集
    			subsets.push_back(subset);//先将子集保留
    			subset.push_back(v);//再将当前对应的a[index]存进子集中
    			subsets.push_back(subset);
    		}
    	}
    	return subsets;
    }

{% endhighlight %}

完整代码：

{% highlight java %}

    #include<iostream>
    #include<vector>
    using namespace std;
    
    typedef vector<vector<int>> vvi;
    typedef vector<int> vi;
    vvi GetSubsets1(int a[], int n)
    {
    	vvi subsets;
    	int max = 1<<n;//1向左移n位，即2的n次方
    	for (int i = 0; i < max; ++i)//从0到2^n-1遍历
    	{
    		vi subset;
    		int j = i;
    		int index = 0;
    		while (j>0)
    		{
    			if (j&1)//判断j的最后一位是否为1，是的话将该位对应的存进向量
    			{
    				subset.push_back(a[index]);
    			}
    			j = j>>1;//j向右移动一位，然后继续循环判断最后一位是否为0
    			++index;
    		}
    		subsets.push_back(subset);
    	}
    	return subsets;
    }
    
    vvi GetSubsets2(int a[], int index, int n)
    {
    	vvi subsets;
    	if (index == n)
    	{
    		vi subset;
    		subsets.push_back(subset);//递归终止条件，空集
    	}else
    	{
    		vvi rsubsets = GetSubsets2(a,index+1,n);//获取递归回来的集合
    		int v = a[index];
    		for (int i=0; i<rsubsets.size(); ++i)
    		{
    			vi subset = rsubsets[i];//获取递归回来的子集
    			subsets.push_back(subset);//先将子集保留
    			subset.push_back(v);//再将当前对应的a[index]存进子集中
    			subsets.push_back(subset);
    		}
    	}
    	return subsets;
    }
    
    void PrintSubsets(vvi subsets)
    {
    	for (int i=0; i<subsets.size(); ++i)
    	{
    		vi subset = subsets[i];
    		for (int j= 0; j<subset.size(); ++j)
    		{
    			cout<<subset[j]<<" ";
    		}
    		cout<<endl;
    	}
    }
    int main()
    {
    	int a[] = {1,2,3,4};
    	vvi subsets1 = GetSubsets1(a,4);
    	vvi subsets2 = GetSubsets2(a,0,4);
    	PrintSubsets(subsets1);
    	PrintSubsets(subsets2);
    	return 0;
    }

{% endhighlight %}

参考资料：

- 作者：Hawstein 出处：[http://hawstein.com/posts/8.3.html](http://hawstein.com/posts/8.3.html)
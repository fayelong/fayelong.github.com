---
layout: post
title: "写一个函数返回一个串的所有全排列"
description: "写一个函数返回一个串的所有全排列"
category: algorithm
tags: [algorithm]
---
{% include JB/setup %}


写一个函数返回一个串的所有全排列,今年参加UC笔试时也刚好是这道题，那时使用第一种方法


思路一：

抽取字符串中的第一个，剩下的进行全排列。直到递归结束

递归结束的条件是到最后一个字符

{% highlight cpp %}

	void Permutation1(char* pStr,char *pBegin)
	{
		if (!pStr||!pBegin)
		{
			return;
		}
		if (*pBegin=='\0')//当递归到最后一个时，为递归的终止条件
		{
			cout<<pStr<<endl;
		}else
		{
			for (char *ch = pBegin;*ch!='\0';++ch)//抽空一个，剩下的进行全排列，递归到最后一个，因此抽空后，需要进行交换
			{
				char temp = *ch;
				*ch = *pBegin;
				*pBegin = temp;
	
				Permutation1(pStr,pBegin+1);
	
				temp = *pBegin;
				*pBegin = *ch;
				*ch = temp;
			}
		}
	}

{% endhighlight %}

思路二：

我们可以把串“abc”中的第0个字符a取出来，然后递归调用permu计算剩余的串“bc” 的排列，得到{bc, cb}。

然后再将字符a插入这两个串中的任何一个空位(插空法)， 得到最终所有的排列。比如，a插入串bc的所有(3个)空位，得到{abc,bac,bca}。 

递归的终止条件是什么呢？当一个串为空，就无法再取出其中的第0个字符了， 所以此时返回一个空的排列。代码如下：

{% highlight cpp %}

	typedef vector<string> vs;
	vs Permutation2(string s)
	{
		vs result;
		if (s=="")
		{
			result.push_back("");
			return result;
		}
		string c = s.substr(0,1);
		vs res = Permutation2(s.substr(1));
		for (int i=0;i<res.size();++i )
		{
			string t = res[i];
			for (int j=0; j<=t.length();++j)
			{
				string u = t;
				u.insert(j,c);
				result.push_back(u);
			}
		}
		return result;
	}

{% endhighlight %}

思路三：（同思路一）

我们还可以用另一种思路来递归解这个问题。还是针对串“abc”， 我依次取出这个串中的每个字符，然后调用permu去计算剩余串的排列。 

然后只需要把取出的字符加到剩余串排列的每个字符前即可。对于这个例子， 程序先取出a，然后计算剩余串的排列得到{bc,cb}，然后把a加到它们的前面，得到 {abc,acb}；接着取出b，计算剩余串的排列得到{ac,ca}，然后把b加到它们前面， 得到{bac,bca}；后面的同理。最后就可以得到“abc”的全序列。代码如下：

{% highlight cpp %}

	vs Permutation3(string s)
	{
		vs result;
		if (s == "")//最后一个
		{
			result.push_back("");
			return result;
		}
		for (int i=0; i<s.size(); ++i)
		{
			string c = s.substr(i,1);//取第i位的字符
			string t = s;//使用一个临时变量保存一会剔掉该位的字符串
			vs res = Permutation3(t.erase(i,1));//t.erase，删除该位，该字符串就短了一个，因此后面要加上.//erase(pos,n); 删除从pos开始的n个字符，比如erase(0,1)就是删除第一个字符
			for (int j = 0;j<res.size();++j)
			{
				result.push_back(c+res[j]);
			}
		}
		return result;
	}

{% endhighlight %}


主函数：

{% highlight cpp %}

	#include <iostream>
	#include<vector>
	#include<string>
	using namespace std;
	
	void Print(vs result)
	{
		for (int i=0; i<result.size();++i)
		{
			cout<<result[i]<<endl;
		}
	}
	
	int main(){
		cout<<"use Permulation1:"<<endl;
		char s1[6]="abcde";
		Permutation1(s1,s1);
		
		cout<<"use Permulation2:"<<endl;
		string s2="abcde";
		vs result2 = Permutation2(s2);
		Print(result2);
	
		cout<<"use Permulation3:"<<endl;
		string s3 = "abcde";
		vs result3 = Permutation3(s3);
		Print(result3);
	
		return 0;
	}

{% endhighlight %}
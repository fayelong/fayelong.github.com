---
layout: post
title: "判断一颗树是否平衡"
description: "实现一个函数检查一棵树是否平衡。对于这个问题而言， 平衡指的是这棵树任意两个叶子结点到根结点的距离之差不大于1。"
category: algorithm
tags: [algorithm]
---
{% include JB/setup %}


实现一个函数检查一棵树是否平衡。对于这个问题而言， 平衡指的是这棵树任意两个叶子结点到根结点的距离之差不大于1。

对于本题，只需要求出离根结点最近和最远的叶子结点， 然后看它们到根结点的距离之差是否大于1即可。

假设只考虑二叉树，我们可以通过遍历一遍二叉树求出每个叶子结点到根结点的距离。 使用中序遍历，依次求出从左到右的叶子结点到根结点的距离，递归实现。
    
    #include <iostream>
    #include <cstring>
    #include <cmath>
    using namespace std;
    
    const int maxn = 100;
    struct Node{
    	int key;
    	Node *lchild;
    	Node *rchild;
    	Node *parent;
    };
    Node *head,*p;
    Node node[maxn];
    int cnt;
    
    void init(){
    	head = p = NULL;
    	memset(node,'\0',sizeof(node));
    	cnt = 0;
    }
    
    void insert(Node* &head,int x){
    	if (head == NULL){
    		node[cnt].key = x;
    		node[cnt].parent = p;
    		head = &node[cnt++];
    		return;
    	}
    	p = head;//notice
    	if (x<head->key){
    		insert(head->lchild,x);
    	}else{
    		insert(head->rchild,x);
    	}
    }
    
    int d = 0, num = 0, dep[maxn];
    
    void GetDepth(Node *head){
    	if (head == NULL){
    		return;
    	}
    	++d;
    	GetDepth(head->lchild);
    	if (head->lchild == NULL && head->rchild == NULL){
    		dep[num++] = d;
    	}
    	GetDepth(head->rchild);
    	--d;
    }
    
    bool IsBalance(Node *head){
    	if (head == NULL)//空树默认平衡
    		return true;
    	GetDepth(head);
    	int max = dep[0], min = dep[0];
    	for (int i = 0; i < num; ++i){
    		if (dep[i]>max)max = dep[i];
    		if (dep[i]<min)min = dep[i];
    	}
    	if ((max - min)>1)return false;
    	else return true;
    }
    
    int main()
    {
    	init();
    	int a[] = {5, 3, 8, 1, 4, 7, 10, 2, 6, 9, 11, 12};
    	for (int i = 0; i < 12; i++){
    		insert(head,a[i]);
    	}
    	cout<<IsBalance(head)<<endl;
    	return 0;
    }

参考资料：

- 作者：Hawstein 出处：[http://hawstein.com/posts/4.1.html](http://hawstein.com/posts/4.1.html)
---
layout: post
title: "线段树总结"
date: 2016-09-01
desc: "线段树总结"
keywords: "C++,Segment Tree"
categories: [Noip]
tags: [C++,Segment Tree]
---

信息学竞赛中，经常会遇到这样一类问题：**多次对数轴上的一个区间或是数列中的连续若干个数进行一种相同的处理(查询、修改等)**。常规做法是依托于线性表这种数据结构，导致只能针对各个元素逐个进行，效率低下。  
**线段树是一种有效处理区间操作的高级数据结构，效率较高。**

## 线段树基础

线段树是二叉搜索树，他将一个区间划分成多个单元区间，每个单元区间对应线段树中的一个叶节点。  
对于线段树中每个非叶节点[a,b]，它左儿子区间为[a,(a + b) / 2]，右儿子区间为[(a + b) / 2 +1,b]。因此线段树是平衡二叉树，最后的叶节点数目为N，即整个线段区间长度。  

### 以求区间最大值为例

* 声明

``` c++
const int MaxNode = 2000000;
const int MAX = 1000000;

struct NODE{
    int nValue;         //节点对应区间 权值
    int nLeft,nRight; //区间[left,right]
}Node[MaxNode];
int nFather[MAX] = {0};
```

* 创建线段树
在完全二叉树中，假如一个节点的Index为l，则：  
l的父亲：l / 2  
l的兄弟：l / 2 * 2 或 l / 2 * 2 + 1  
l的孩子：l * 2 和 l * 2 + 1  

``` c++
//在区间[nLeft,nRight]建立一个以nIndex为祖先的线段树
void BuildTree(int nIndex,int nLeft,int nRight){
    Node[nIndex].nLeft = nLeft;
    Node[nIndex].nRight = nRight;
    Node[nIndex].nValue = 0;
    if(nLeft == nRight){                   //到达叶节点时结束递归
        nFather[nLeft] = nIndex;
        return ;
    }
    
    int nMid = (nLeft + nRight) >> 1;
    BuildTree(nMid << 1,nLeft,nMid);
    BuildTree(nMid << 1 | 1,nMid + 1,nRight);
}
```

* 单点更新线段树(自下而上更新)

``` c++
//下标为 nIndex 的节点在函数外已经更新
void UpdateTree(int nIndex){
    if(nIndex == 1) return ;
    
    int nFather = nIndex / 2;
    int nValue_Left = Node[nFather << 1].nValue;
    int nValue_Right = Node[nFather << 1 | 1].nValue;
    
    Node[nFather].nValue = (nValue_Left > nValue_Right) ? nValue_Left : nValue_Right;
    UpdateTree(nFather);
}
```

* 查询区间最大值(自上而下查询)

``` c++
int nMax = -‭1073741823‬;
//查询区间[nLeft,nRight]的最大值
void QueryTree(int nIndex,int nLeft,int nRight){
    if(Node[nIndex].nLeft == nLeft && Node[nIndex].nRight == nRight){
        nMax = max(nMax,Node[nIndex].nValue);
        return ;
    }
    
    nIndex <<= 1;
    if(nLeft <= Node[nIndex].nRight){
        if(nRight <= Node[nIndex].nRight){
            QueryTree(nIndex,nLeft,nRight);
        }else{
            QueryTree(nIndex,nLeft,Node[nIndex].nRight);
        }
    }
    
    nIndex += 1;
    if(nRight >= Node[nIndex].nRight){
        if(nLeft >= Node[nIndex].nLeft){
            QueryTree(nIndex,nLeft,nRight);
        }else{
            QueryTree(nIndex,Node[nIndex].nLeft,nRight);
        }
    }
}
```

## 线段树优化

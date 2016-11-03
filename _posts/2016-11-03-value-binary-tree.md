---
layout: post
title: "【Vijos 1100】 加分二叉树"
date: 2016-11-03
tags: [C++, Noip, Tree, DP]
categories: [Noip]
bgp: "bg_text_3"
---

题目链接： [加分二叉树](https://vijos.org/p/1100) <small>2003年NOIP全国联赛提高组</small>  

## 思路

首先想到搜索， 显然会超时(╮(╯▽╰)╭)， 因为没有什么强有力的剪枝(或者我没有想到)。  
考虑 `DP(树形DP, 区间划分)` ：  
`阶段` ： 一棵以 i 为根的树的最大分数满足条件为 ： **左子树最大分数 × 右子树最大分数 + 根i的分数**， 我们以中序遍历区间划分阶段， 一棵以i(i∈划分区间)为根的子树为一个阶段。 因为题目给的是`中序遍历`， 这保证所给节点都是相邻的， 保证了以区间划分子树的正确性。  
`状态` ： 在"阶段"的基础上， 申请数组 F[i][j] 表示中序遍历中 i~j 区间的最大分数. 显然的， F[i][i] = A[i].  
`转移` ： F[i][j] = max(F[i][j], F[i][k -  1] * F[k + 1][j] + F[k][k]) (k∈[i, j]);  
**DP 使用记忆化进行优化， 已经计算出F[i][j]就直接使用， 无需再次计算**  

处理输出先序遍历问题:  
申请数组 `root[i][j]` 表示区间[i, j]的子树的根， 更新F[i][j]的同时更新root[i][j]， 最后按照`左根右`顺序递归输出 root[][]。  

## 代码

```c++
/*
* Copyright © Eliot
* Date: 2016-11-03
* Author: Eliot
* QQ: 1161142536
* Description: 树形DP 区间划分
*/

#include <iostream>
#include <cstring>
#include <cstdio>
using namespace std;

int n = 0;
long F[40][40] = { 0 };
int root[40][40] = { 0 };
void visit(int i, int j) {
	if(i > j) return;
	else {
		cout << root[i][j] << " ";
		visit(i, root[i][j] - 1);
		visit(root[i][j] + 1, j);
	}
}
long Search(int l, int r) {
    long now = 0;
    if(l > r) {
      	return 1;
	}
    else {
        //记忆化
    	if(F[l][r] == 0) {
    	    //枚举根
            for(int i = l; i <= r; i ++) {
                now = Search(l,i - 1) * Search(i + 1, r) + F[i][i];
                if(now > F[l][r]) {
                    F[l][r] = now;
                    root[l][r] = i;
                }
            }
        }
        return F[l][r];
    }
}
int main() {
	scanf("%d", &n);
 	for(int i = 1; i <=  n; i ++) {
	  scanf("%d", &F[i][i]);
      root[i][i] = i;
    }
 	cout << Search(1, n) << endl;
 	visit(1, n);

 	return 0;
}
```

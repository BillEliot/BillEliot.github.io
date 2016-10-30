---
layout: post
title: "【题目】宿命的PSS 逆向Kruskal 并查集"
date: 2016-10-25
desc: "宿命的PSS 逆向Kruskal 并查集"
keywords: "C++, Noip, Kruskal"
tags: [C++, Noip, Kruskal]
categories: [Noip]
bgp: "bg_text_4"
---

**题目链接**：[宿命的PSS](https://vijos.org/p/1579)  

## 定义

```最小生成树``` ：一个有 n 个结点的连通图的生成树是原图的极小连通子图，且包含原图中的所有 n 个结点，并且有保持图连通的最少的边。  
```完全图``` ：若一个图的每一对不同顶点恰有一条边相连，则称为完全图。完全图是每对顶点之间都恰连有一条边的简单图。  
```最小完全图``` :边权之和最小的完全图。  

## 讲解

题目要求基于给定的最小生成树求出最小完全图， 考虑Kruskal的逆向算法。  
按照Kruskal的算法思想：加入一条边之前，其连接的两点(a, b)分属不同的两个集合，且该边边权为连接这两个集合的最小边权(唯一的最小生成树)， 构建最小完全图：  
**在这两个集合间连接n条边权为m的边**  
**n: a点所在集合顶点数 × b点所在集合顶点数 - 1**  
**m: 当前边权+1**  
答案便是构造的最小完全图中所有边权之和。  

## 代码

```c++
/*
* Copyright © Eliot
* Date: 2016-10-25
* Description: 逆向Kruskal， 并查集
* Author: Eliot
* QQ: 1161142536
*/

#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;

#define MAX 200001

long long ans = 0;
long long n = 0;
long long a = 0, b = 0, c = 0;

struct LINE{
	long long a;
	long long b;
	long long c;
	LINE() : a(0), b(0), c(0) {}
}Line[MAX];

int comp(LINE A, LINE B) {
	return A.c < B.c;
}

long long nFather[MAX] = { 0 };
long long Num[MAX] = { 0 };
void Init() {
	for (int i = 1; i <= MAX; i ++) {
		nFather[i] = i;
		Num[i] = 1;
	}
}
long long Find(long long x) {
	if (nFather[x] != x) {
		nFather[x] = Find(nFather[x]);
	}
	return nFather[x];
}

void Union(long long nFather_A, long long nFather_B) {
	nFather[nFather_A] = nFather_B;
	Num[nFather_B] += Num[nFather_A];
}

int main () {
	Init();
	
	scanf("%lld", &n);
	for (int i = 1; i <= n - 1; i ++) {
		scanf("%lld %lld %lld", &a, &b, &c);
		
		Line[i].a = a;
		Line[i].b = b;
		Line[i].c = c;
		ans += c;
	}
	sort(Line + 1, Line + n, comp);
	
	for (int i = 1; i <= n - 1; i ++) {
		LINE L = Line[i];
		long long nFather_A = Find(L.a);
		long long nFather_B = Find(L.b);		
		
		ans += (Num[nFather_A] * Num[nFather_B] - 1) * (L.c + 1);
		Union(nFather_A, nFather_B);
	}
	
	cout << ans << endl;
	return 0;
} 
```

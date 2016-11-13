---
layout: post
title: "【金陵 图论专题】删边问题"
date: 2016-11-14
tags: [C++, Noip, Graph]
categories: [Noip]
bgp: "bg_text_20"
---

```
题目描述：连通图是指任意两个顶点都有路径可互相到达的图。
读入一个无向的连通图，输出最多能删掉多少条边，使这个图仍然连通。
输入格式：
第一行为图的顶点数N（1<=N<=100）和边数M，它们之间用一个空格隔开，图中的顶点用1到N的整数标号。接下来的M行，每行用两个数v1和v2表示一条边。v1和v2用一个空格隔开，表示这条边所连接的顶点的标号（v1<>v2），同一条边不会重复出现。
输出格式：
输出最多能删掉的边数。

输入
5 6
4 6
1 2
1 3
1 4
2 3
2 4
3 4	
输出
3
```

## 思路

这题其实是一道脑残题, 仔细思考就好了.  
题目给的是一个无向连通图, 要我们删去最多的边后仍保证该图是一个连通图, 即留下最少的边, 最少的边仍保证连通, 那我们就只留下点与点之间的边, 其他全删掉, 这样图肯定是连通的, 因为题目所给的图就是连通的, 那么答案就是 m - (n - 1).  

## 代码

```c++
/*
* Copyright © Eliot
* Date: 2016-11-14
* Author: Eliot
* QQ: 1161142536
* Description: Water
*/

#include <iostream>
#include <cstdio> 
using namespace std;

class EDGES{
public:
	EDGES() : N(0), M(0), temp(0) {};
	~EDGES() {};
public:
	void Read() {
		scanf("%d %d", &N, &M);
		for (int i = 1; i <= M; i++) {
			scanf("%d", &temp);
		}
	}
	void DO() {
		printf("%d\n", M - (N - 1));
	}
private:
	int N, M, temp;
};

EDGES E;
int main() {
	E.Read();
	E.DO();
	
	return 0;
}
```

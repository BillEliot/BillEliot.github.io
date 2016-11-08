---
layout: post
title: "【CODEVS 1183】泥泞的道路"
date: 2016-11-08
tags: [C++, Noip, Binary, Modaling]
categories: [Noip]
bgp: "bg_text_19"
---

## 思路

这道题既不是求路程最优值, 也不是求时间最优值, 而是求`速度最优值`, 乍一看还以为是最优比率生成树, 感觉难度又太高了, 不像。 仔细想想算算, 是一个隐藏着的`二分+SPFA`。  
题目要求的是从起点到终点的速度的最大值, 我们设 ans = 总路程/总时间, 那么确定的一条道路 ans = {s<sub>1</sub> + s<sub>2</sub> + ... + s<sub>n</sub>} / {t<sub>1</sub> + t<sub>2</sub> + ... + t<sub>n</sub>}, 对式子变形:  
=> ans * {t<sub>1</sub> + t<sub>2</sub> + ... + t<sub>n</sub>} = {s<sub>1</sub> + s<sub>2</sub> + ... + s<sub>n</sub>}  
=> (ans * t<sub>1</sub> - s<sub>1</sub>) + (ans * t<sub>2</sub> - s<sub>2</sub>) + (ans * t<sub>n</sub> - s<sub>n</sub>) = 0  
我们以 (ans * t<sub>n</sub> - s<sub>n</sub>) 为边权重新构图, 二分答案(速度)ans, 当 ans < {s<sub>1</sub> + s<sub>2</sub> + ... + s<sub>n</sub>} / {t<sub>1</sub> + t<sub>2</sub> + ... + t<sub>n</sub>}, 即 (ans * t<sub>1</sub> - s<sub>1</sub>) + (ans * t<sub>2</sub> - s<sub>2</sub>) + (ans * t<sub>n</sub> - s<sub>n</sub>) < 0 时, 向上继续二分, 否则向下继续二分.  
Check就是跑一遍 SPFA最短路, 当 dis[n] <= 0 成立, 否则不成立.  
**数据坑爹, 需要要判断负环**  

```c++
/*
* Copyright © Eliot
* Date: 2016-11-08
* Author: Eliot
* QQ: 1161142536
* Description: 建模 + 二分 + SPFA最短路
*/

#include <iostream>
#include <cstdio>
#include <queue>
#include <cstring>
using namespace std;

const int MAX = 101;

int n = 0, temp = 0;
double *pLeft = 0, *pRight = 0;
double Arr[MAX][MAX] = { 0 };

double dis[MAX] = { 0 };
bool IsStack[MAX] = { false };
int Count[MAX] = { 0 };
bool SPFA() {
	queue<int> Q;
	for (int i = 0; i < MAX; i++) dis[i] = 9999999;
	bool IsStack[MAX] = { false };
	int Count[MAX] = { 0 };
	
	Q.push(1);
	IsStack[1] = true;
	dis[1] = 0;
	while (!Q.empty()) {
		int nPoint = Q.front();
		Q.pop();
		IsStack[nPoint] = false;
		for (int i = 1; i <= n; i++) {
			if (dis[nPoint] + Arr[nPoint][i] < dis[i]) {
				dis[i] = dis[nPoint] + Arr[nPoint][i];
				if (!IsStack[i]) {
					Count[i] ++;
					if (Count[i] > 3000) return true;
					IsStack[i] = true;
					Q.push(i);
				}
			}
		}
	}

	if (dis[n] <= 0) {
		return true;
	}
	else {
		return false;
	}
}

int main() {
	int(*pArr_dis)[MAX] = new int[MAX][MAX];
	int(*pArr_time)[MAX] = new int[MAX][MAX];
	pLeft = new double(0);
	pRight = new double(100000);

	scanf("%d", &n);
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			scanf("%d", &temp);
			pArr_dis[i][j] = temp;
		}
	}
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			scanf("%d", &temp);
			pArr_time[i][j] = temp;
		}
	}

	while (*pRight - *pLeft > 0.0001) {
		double dMid = (*pLeft + *pRight) / 2.0;

		//重新构图
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= n; j++) {
				Arr[i][j] = pArr_time[i][j] * dMid - pArr_dis[i][j];
			}
		}

		if (SPFA()) {
			*pLeft = dMid;
		}
		else {
			*pRight = dMid;
		}
	}
	printf("%.3f\n", *pLeft);

	pArr_time = 0; pLeft = 0;
	pArr_dis = 0; pRight = 0;
	delete[] pArr_time; delete pLeft;
	delete[] pArr_dis; delete pRight;

	return 0;
}
```

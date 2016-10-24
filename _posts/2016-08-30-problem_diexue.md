---
layout: post
title: "【建模】Vijos P1767 YYB喋血"
date: 2016-10-24
desc: "【建模】Vijos P1767 YYB喋血"
keywords: "C++, Noip, Modeling"
categories: [Database]
---

题目链接：[YYB喋血](https://vijos.org/p/1767)  

**思路**

这种题目就是考的```建模能力```， 算法并不难， 就一个```SPFA```。  
看完题目， 第一反应就是改变和治愈点相连的边的权值， 但又一想又不对， 治愈点是清空喋血值， 并非改变部分喋血值。  
仔细想想， 每次到治愈点就会将喋血值清空， 这就相当于一个起点， 所以总共有 k + 1 个起点， 要做 k + 1 次SPFA， 第一次判断连通性， 不连通就输出 “Oh no!”， 否则做剩下的 k 次SPFA， 求dis[n]的最小值即为答案。  

**代码**

```c++
/*
* Copyright © Eliot
* Date: 2016-10-24
* Author: Eliot
* Description: 建模能力 + SPFA
* QQ: 1161142536
*/

#include <iostream>
#include <cstdio>
#include <queue>
#include <cstring>
using namespace std;

#define MAX 5001

int nCure[MAX] = { 0 };

struct STRUCT{
	int nPre;
	int nTo;
	int nVal;
	STRUCT () : nPre(0), nTo(0), nVal(0) {}
}EDGE[100 * MAX];
int head[MAX] = { 0 };
int nCount = 0;

void Add(int a, int b, int c) {
	EDGE[++ nCount].nTo = b;
	EDGE[nCount].nVal = c;
	EDGE[nCount].nPre = head[a];
	head[a] = nCount;
}

queue<int> Q;
int dis[MAX] = {0};
bool IsStack[MAX] = { false };
void SPFA(int nStart) {
	memset(dis, 0x7f, sizeof(dis));
	
	Q.push(nStart);
	dis[nStart] = 0;
	IsStack[nStart] = true;
	
	while (!Q.empty()) {
		int nPoint = Q.front();
		Q.pop();
		IsStack[nPoint] = false;
		
		for (int i = head[nPoint]; i != 0; i = EDGE[i].nPre) {
			int nNextPoint = EDGE[i].nTo;
			
			if (dis[nPoint] + EDGE[i].nVal < dis[nNextPoint]) {
				dis[nNextPoint] = dis[nPoint] + EDGE[i].nVal;
				if (!IsStack[nNextPoint]) {
					Q.push(nNextPoint);
					IsStack[nNextPoint] = true;
				}
			}
		}
	}
} 

int ans = 0x7fffffff;

int n = 0, m = 0, k = 0;
int x = 0, y = 0, z = 0;
int main() {
	scanf("%d %d %d", &n, &m, &k);
	for (int i = 1; i <= m; i ++) {
		scanf("%d %d %d", &x, &y, &z);
		Add(x, y, z);
		Add(y, x, z);
	}
	for (int i = 1; i <= k; i ++) {
		scanf("%d", &nCure[i]);
	}
	
	SPFA(1);
	if (dis[n] == 2139062143){
		printf("Oh no!\n");
		return 0;
	}
	else {
		for (int i = 1; i <= k; i ++) {
			memset(dis, 0x7f, sizeof(dis));
			memset(IsStack, false, sizeof(IsStack));
			
			SPFA(nCure[i]);
			ans = min(ans, dis[n]);
		}
	}
	
	cout << ans << endl;
	return 0;
}
```

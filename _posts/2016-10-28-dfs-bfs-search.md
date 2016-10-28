---
layout: post
title: "【搜索】密室逃脱"
date: 2016-10-27
desc: "【搜索】密室逃脱"
keywords: "C++, Noip, Search"
tags: [C++, Noip, Search]
categories: [Noip]
---

## 题目

```
                                            密室逃脱
即使czhou没有派出最强篮球阵容，机房篮球队还是暴虐了校篮球队。为了不打击校篮球队信心，czhou决定改变训练后的活动。近来，江大掌门的徒弟徒孙们纷纷事业有成，回到母校为机房捐钱捐物。财大气粗的机房组收回了五层六层的所有教室。Czhou决定将六层的教室改造为智能密室逃脱活动室。

    每天傍晚，神牛们可以依次逐个进入游玩。我们简单的将教室分割为n*n个房间，K是你初始所在房间，T是你最终逃脱的房间。如果你想要逃脱房间，你必须依次找到m把钥匙。我们假定你从一个房间进入另一个房间需要花费1的时间。当然某些房间有些特殊的问题(地图上S表示)需要回答才能通过，对于机智的众牛们来说，这些问题根本不是问题。我们假定众牛们花费1的时间解决问题。（主要是出题的人表述不清，导致众牛理解困难；当然问题只需要回答一次，下次再次进入房间不需要回答了）

Input：maze.in
第一行两个数字n，m
接下来n*n描述地图
Output: maze.out
需要最少时间逃脱密室。若无解输出impossible
Sample1.in:
3 1 
K.S 
##1 
1#T
Sample1.out
5
-----------
Sample2.in
3  1
K#T
.S# 
1#.
Sample2.out
Impossible
-----------
Sample3.in
3 2 
K#T 
.S. 
21.
Sample3.out
8
```

## 反思

一道很好的搜索题, 值得一做。最开始看到这题， 犯了经验主义， 导致第三个样例看了好久才反应过来:   

* 因为题目中说依次获取钥匙(1、2、3 ...)， 于是我便经验地认为必须先获取1钥匙, 再获取2钥匙 。。。， 其实不然， 在你没有获取1钥匙之前，也是可以到达2钥匙位置的， 只不过之后在返回获取1钥匙， 折回获取2钥匙罢了。  
* 理所当然的认为必须走S点(╮(╯▽╰)╭)， 其实不走也是可以的。。。  

**总之, 细心分析题意就对了**  

## 思路

采用 DFS+BFS 的搜索策略: DFS枚举要经过的S点, BFS来求 K->T 的最短路。  
BFS的时候, 申请 `F[x][y][t]` 数组来保存自起点到 `(x, y)` 且当前钥匙为t时的最短路, 解决带条件搜索的问题。  
答案就是最短路中的最小值。  

*应该可以状态压缩, 加强版的貌似是 [拯救公主](http://noi.openjudge.cn/ch0205/7221/)*

## 代码

```c++
/*
* Copyright © Eliot
* Date: 2016-10-28
* Author: Eliot
* QQ: 1161142536
* Description: DFS+BFS 带条件搜索
*/

#include <iostream>
#include <queue>
#include <cstring>
using namespace std;

const int INF = 0x7fffffff;
const int xx[5] = { 0, 0, 0, 1, -1 };
const int yy[5] = { 0, 1, -1, 0, 0 }; 

struct NODE {
	int x;
	int y;
	int t;
}Temp;

int n = 0, m = 0;
int kx = 0, ky = 0, tx = 0, ty = 0, cnt = 0, ans = INF;
char A[105][105] = { 0 };
int x[10] = { 0 }, y[10] = { 0 };
int F[105][105][10] = { 0 };

void BFS() {
	memset(F, -1, sizeof(F));
	queue<NODE> Q;
	
	Temp.x = kx; Temp.y = ky; Temp.t = 0;
	Q.push(Temp);
	F[kx][ky][0] = 0;
	
	while(!Q.empty()) {
		NODE q = Q.front();
		Q.pop();
		
		for (int i = 1; i <= 4; i ++) {
		    int x = q.x + xx[i];
			int y = q.y + yy[i];
			int t = q.t;
		     
		    if (x < 1 || x > n || y < 1 || y > n) continue;
		    if (A[x][y] == '#' || F[x][y][t] != -1) continue;
		    if (t + 1 == A[x][y] - '0') t ++;
		    
		    F[x][y][t] = F[q.x][q.y][q.t] + 1;
		    Temp.x = x; Temp.y = y; Temp.t = t;
			Q.push(Temp);
		 }
	}
}

void DFS(int now, int k) {
	if(now == cnt + 1) {
		BFS();
		if(F[tx][ty][m] != -1)
		    ans = min(ans, k + F[tx][ty][m]);
		 	return;
	}
	A[x[now]][y[now]] = '.';
	DFS(now + 1, k + 1);
	A[x[now]][y[now]] = '#';
	DFS(now + 1, k);
}

int main() {
	cin >> n >> m;
	for (int i = 1; i <= n; i ++) {
	    scanf("%s", A[i] + 1);
     	for(int j = 1; j <= n; j ++) {
	        switch(A[i][j]) {
	        case 'K':
	           kx = i;
			   ky = j;
	           break;
	        case 'T':
	           tx = i;
			   ty = j;
	           break;
	        case 'S':
	            cnt ++;
	            x[cnt] = i; 
				y[cnt] = j;
	            break;
	        }
	    }
	}
	DFS(1, 0);
	if(ans == INF) {
		cout << "Impossible" << endl;
	}
	else {
		cout << ans << endl;
	}
	
	return 0;
}
```

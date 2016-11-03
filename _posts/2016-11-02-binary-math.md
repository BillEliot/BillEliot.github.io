---
layout: post
title: "【智障模拟赛】 智障的小Q"
date: 2016-11-02
tags: [C++, Noip, Binary]
categories: [Noip]
bgp: "bg_text_8"
---

```
【问题描述】 
小 Q 对计算几何有着浓厚的兴趣。他经常对着平面直角坐标系发呆，思考 一些有趣的问题。今天，他想到了一个十分有意思的题目： 首先，小 Q 会在x轴正半轴和y轴正半轴分别挑选n个点。随后，他将x轴的 点与y轴的点一一连接，形成n条线段，并保证任意两条线段不相交。小 Q 确定 这种连接方式有且仅有一种。最后，小 Q 会给出m个询问。对于每个询问，将会 给定一个点P(x, y)，请回答线段 OP 与n条线段会产生多少个交点？ 小 Q 找到了正在钻研数据结构的你，希望你可以帮他解决这道难题。 
【输入格式】 
第1行包含一个正整数n，表示线段的数量； 第2行包含n个正整数，表示小 Q 在x轴选取的点的横坐标； 第3行包含n个正整数，表示小 Q 在y轴选取的点的纵坐标； 第 4 行包含一个正整数m，表示询问数量； 随后m行，每行包含两个正整数x, y，表示询问中给定的点的横、纵坐标。
 
【输出格式】 
共m行，每行包含一个非负整数，表示你对这条询问给出的答案。 
【样例输入】 
3 
4 5 3 
3 5 4 
2
1 1 
3 3 
【样例输出】 
0 3 
【数据规模与约定】 
对于50%的数据，1 ≤ n,m,≤ 2 * 10^3。 对于100%的数据，1 ≤ n,m ≤ 2 * 10^5，坐标范围 ≤ 10^8。 
```

## 思路

很明显的， 因为题目中说连接的线段两两不相交， 所以将 x坐标 和 y坐标 `排序`之后依次相连就好了。  判断 OP 和几条线段相交利用数学知识就好 ： 判断出P点在第n条线段上面， 那么相交线段就是n条。  

我最开始暴力枚举， 枚举线段从n到1， 只要P点在第n条线段之上， 则输出n，退出. 结果超时(显然╮(╯▽╰)╭)。  
考虑优化：利用`二分`进行优化， 二分n条线段， 降低时间复杂度。  

## 注意事项

* double 的计算问题

看以下两种计算方式：  
一  

```c++
double nL_x = A[nIndex], nL_y = B[nIndex];
double k = -(nL_y / nL_x);
```

二  

```c++
int nL_x = A[nIndex], nL_y = B[nIndex];
double k = -((double)nL_y / (double)nL_x);
```

我们使用第一种计算方式， 而不使用强制转换(说多都是泪), 貌似会造成精度丢失。  

* 二分的正确写法

如果二分写的不对的话， 那就哭去吧， 各种 多1 少1 。。。。。。  

```c++
int l = 0, r = n;
while (l <= r) {
	int nMid = (l + r) >> 1;
	if (Judge(x, y, nMid)) {
		l = nMid + 1;
	}
	else {
		r = nMid - 1;
	}
}

cout << l - 1 << endl;
```

## 代码

```c++
/*
* Copyright © Eliot
* Date: 2016-11-02
* Author: Eliot
* QQ: 1161142536
* Description: 论二分正确的重要性
*/

#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;

const int MAX = 200001;
int A[MAX] = { 0 }, B[MAX] = { 0 };

bool Judge(double x, double y, int nIndex) {
	double nL_x = A[nIndex], nL_y = B[nIndex];

	double k = -(nL_y / nL_x);
	double b = nL_y;
	double ans_y = k * x + b;
	if (ans_y <= y) {
		return true;
	}
	else {
		return false;
	}
}

int main() {
	int n = 0, m = 0, x = 0, y = 0;

	scanf("%d", &n);
	for (int i = 1; i <= n; i++) scanf("%d", &A[i]);
	for (int i = 1; i <= n; i++) scanf("%d", &B[i]);
	sort(A + 1, A + n + 1);
	sort(B + 1, B + n + 1);

	scanf("%d", &m);
	int l = 0, r = n;
	for (int i = 1; i <= m; i++) {
		scanf("%d %d", &x, &y);
		
		l = 0, r = n;
		while (l <= r) {
			int nMid = (l + r) >> 1;
			if (Judge(x, y, nMid)) {
				l = nMid + 1;
			}
			else {
				r = nMid - 1;
			}
		}
		
		cout << l - 1 << endl;
	}

	return 0;
}
```

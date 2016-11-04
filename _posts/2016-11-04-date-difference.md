---
layout: post
title: "【智障模拟赛】 智障的日期"
date: 2016-11-04
tags: [C++, Noip, Date]
categories: [Noip]
bgp: "bg_text_16"
---

```
【题目描述】
给你两个日期，问这两个日期差了多少毫秒。
【输入格式】
两行，每行一个日期，日期格式保证为“YYYY-MM-DD hh:mm:ss”这种形式。第二个日期时间一定比第一个日期时间要大两个日期的年份一定都是21世纪的年份。
【输出格式】
一行一个整数代表毫秒数。
【样例输入1】
2000-01-01 00:00:00
2000-01-01 00:00:01
【样例输出1】
1000
【样例输入2】
2000-01-01 00:00:00
2000-11-11 00:00:00
【样例输出2】
27216000000
```

## 思路

这题没什么好说的, 做法全在代码中了, 看看就懂, 其实细心就好。  

## 问题

看下面两个版本的代码：  
一：  

```c++
long long ans = 0;
int nC_D = 36524;

ans += nC_D * 24 * 3600;
```

二：  

```c++
long long ans = 0;
int nC_D = 36524;

ans += nC_D * 24;
ans *= 3600;
```

问题来了 : 第一个 ans 会爆`负数`(但是没有超 long long)， 但第二个就不会。  
求解释缓冲区的存储过程和计算流程！

## 代码

```c++
#include <iostream>
#include <string>
#include <stdlib.h>
using namespace std;

//平年和闰年的月份天数
const int nM_P[13] = { 0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
const int nM_R[13] = { 0, 31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };

string str1, str2;
int Y_1 = 0, M_1 = 0, D_1 = 0, h_1 = 0, m_1, s_1 = 0;
int Y_2 = 0, M_2 = 0, D_2 = 0, h_2 = 0, m_2, s_2 = 0;

bool Is = false;    //false 平 ; true 润
int nC_D = 0;
long long ans = 0;

bool Judge(int Year) {
	if ((Year % 100 == 0 && Year % 400 == 0) || (Year % 100 != 0 && Year % 4 == 0)) {
		return true;
	}
	else {
		return false;
	}
}

int main() {
	getline(cin, str1);
	getline(cin, str2);

	//分解时间_1
	Y_1 = atoi(str1.substr(0, 4).c_str()); str1 = str1.substr(5, str1.length() - 5);
	M_1 = atoi(str1.substr(0, 2).c_str()); str1 = str1.substr(3, str1.length() - 3);
	D_1 = atoi(str1.substr(0, 2).c_str()); str1 = str1.substr(3, str1.length() - 3);
	h_1 = atoi(str1.substr(0, 2).c_str()); str1 = str1.substr(3, str1.length() - 3);
	m_1 = atoi(str1.substr(0, 2).c_str()); str1 = str1.substr(3, str1.length() - 3);
	s_1 = atoi(str1.c_str());
	//分解时间_2
	Y_2 = atoi(str2.substr(0, 4).c_str()); str2 = str2.substr(5, str2.length() - 5);
	M_2 = atoi(str2.substr(0, 2).c_str()); str2 = str2.substr(3, str2.length() - 3);
	D_2 = atoi(str2.substr(0, 2).c_str()); str2 = str2.substr(3, str2.length() - 3);
	h_2 = atoi(str2.substr(0, 2).c_str()); str2 = str2.substr(3, str2.length() - 3);
	m_2 = atoi(str2.substr(0, 2).c_str()); str2 = str2.substr(3, str2.length() - 3);
	s_2 = atoi(str2.c_str());

	//计算年 
	for (int i = Y_1; i < Y_2; i ++) {
		if (Judge(i)) {
			nC_D += 366;
		}
		else {
			nC_D += 365;
		}
	}
	if (Judge(Y_2)) Is = true;
	//计算月
	if (M_2 > M_1) {
		for (int i = M_1; i < M_2; i++) {
			if (Is == true) {
				nC_D += nM_R[i];
			}
			else {
				nC_D += nM_P[i];
			}
		}
	}
	else {
		for (int i = M_2; i < M_1; i++) {
			if (Is == true) {
				nC_D -= nM_R[i];
			}
			else {
				nC_D -= nM_P[i];
			}
		}
	}
	//计算天
	if (D_1 <= D_2) {
		nC_D += D_2 - D_1;
	}
	else {
		nC_D -= D_1 - D_2;
	}
	cout << nC_D << endl; 
	
	//额。。 玄学问题
	ans += nC_D * 24;
	ans *= 3600;
	//计算时
	if (h_1 <= h_2) {
		ans += (h_2 - h_1) * 3600;
	}
	else {
		ans -= (h_1 - h_2) * 3600;
	}
	//计算分
	if (m_1 <= m_2) {
		ans += (m_2 - m_1) * 60;
	}
	else {
		ans -= (m_1 - m_2) * 60;
	}
	//计算秒
	if (s_1 <= s_2) {
		ans += s_2 - s_1;
	}
	else {
		ans -= s_1 - s_2;
	}

	if (ans == 0) {
		printf("0\n");
	}
	else {
		printf("%lld000\n", ans);
	}
	return 0;
}
```

---
layout: post
title: "【CODEVS 1394】 数字串"
date: 2016-11-02
tags: [C++, Noip]
categories: [Noip]
bgp: "bg_text_20"
---

**题目链接**：[数字串](http://codevs.cn/problem/1394/)  

## 思路

本以为是一道DP题， 结果是一道乱搞题， 关键就是如何想到 `优化` 。  
开始很容易的就想到暴力枚举： 确定一个左边界开始向右扫， 当找到所有 1-m 数字后， 比较答案和当前区间并更新。 但这样会进行很多无用的计算， 因为我们扫描了很多重复的区间， 考虑可不可以像KMP那样直接利用前面已经扫描过的区间。  

**建议题解与代码联合起来看， 题解不好描述思路， 见谅！**
我们YY一个数组(注意是YY) `F[]` ， F[i] 表示以第i个数为左边界， 向右包含 1-m 个数的 右边界， 比如对于序列 1 2 2 3 1， F[1] = 4, F[2] = 5, F[3] = 5, F[4]和F[5]为无穷.  
同时申请数组 nCount[] 来保存 1-m 每个数出现的次数， 当 nCount[i] 由0变为1 时，就表明找到了一个新的数， 当次数等于m时就找到了 1-m 个数。  
这样我们只需要扫描一遍 F[1], 之后在F[1]的基础上向后扫描， 一般的， F[i]在F[i-1]的基础上向后扫描。  
可以将F[i]作为右指针， pLeft作为左指针，每次比较当前答案和 F[i]-pLeft+1 即可。

比如对于序列 1 5 5 1 5 3 2 4 1 4， F[1] = 8  j = 1， 利用 nCount[] 维护这个过程， 当 j = 4 时， 得到区间 1 5 3 2 4 ， 之后右指针 F[] 右移到9， 左指针利用 nCount[] 来确定下一个区间。

## 代码

```c++
/*
* Copyright © Eliot
* Date: 2016-11-02
* Author: Eliot
* QQ: 1161142536
* Description: 巧妙的乱搞题
*/

#include <iostream> 
#include <cstdio>

using namespace std;
int nCount[200005] = { 0 }, S = 0;
int A[200005] = { 0 };
int n = 0, m = 0, ans = 1 << 30, t = 0;

int main(){
    scanf("%d %d", &n, &m);
    for(int i = 1; i <= n; i ++) scanf("%d", &A[i]);
    
    //计算 F[1](pRight 相当于 F[]) 
	int pRight = 0;
    while (S < m && pRight <= n) {
    	pRight ++;
    	nCount[A[pRight]] ++;
		if(nCount[A[pRight]] == 1) S ++;
	}
	if (pRight > n) {
		printf("NO\n");
		return 0;
	}
	
	//确定最小区间 
	int pLeft = 1;
	ans = pRight;
    while (pRight <= n) {
        nCount[A[pRight]] ++;
		while (nCount[A[pLeft]] > 1) {
        	nCount[A[pLeft]] --;
			pLeft ++;
		}
		ans = min(ans, pRight - pLeft + 1);
        pRight ++;
    }
    
    printf("%d\n",ans);
    return 0;
}
```

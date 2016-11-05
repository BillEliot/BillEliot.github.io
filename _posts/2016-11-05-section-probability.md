---
layout: post
title: "【智障模拟赛】 智障的区间概率"
date: 2016-11-05
tags: [C++, Noip, Section, Probability]
categories: [Noip]
bgp: "bg_text_17"
---

```
【问题描述】
有N个数，随机选择一段区间，如果这段区间的所有数的平均值在[l, r]中则
你比较厉害。求你比较厉害的概率。
【输入格式】
第一行有三个数N, l, r，含义如上描述。
接下来一行有N个数代表每一个数的值。
【输出格式】
输出一行一个分数a/b
代表答案，其中a, b互质。 如果答案为整数则直接输出该
整数即可。
【样例输入 1】
4 2 3
3 1 2 4
【样例输出 1】
7/10
【样例输入 2】
4 1 4
3 1 2 4
【样例输出 2】
1
【数据规模与约定】
对于30%的数据， 1 ≤ N ≤ 10^4。
对于60%的数据， 1 ≤ N ≤ 10^5。
对于100%的数据， 1 ≤ N ≤ 5 × 10^5, 0 < l ≤ r ≤ 100。
```

## 思路

基于以下结论：  
求区间平均值在[L, R]之间的个数。  
`[L <= x <= R] = [1 <= x <= R] − [1 <= x <= L − 1]`，  
=> 求区间平均值小于等于某个数R的区间个数即可, 把所有数都减去R，  
=> 求区间和小于等于0的区间个数。  
令 sum<sub>0</sub> = 0， sum<sub>i</sub> = ∑a<sub>k</sub>[1 <= k <= i] (前缀和),  
=> 求满足 sum<sub>i</sub> >= sum<sub>j</sub>(0 <= i < j <= n)的个数.  

对于样例, 我们进行如下操作(结合下面代码看, 多看几遍就好)：  

n = 4 l = 2 r = 3  
A[] = 3 1 2 4  

**将 A[] - l**  
得　　　　　　　　　　　　　　　　　　　Z[] = 1 -1 0 2  
求Z[]前缀和　　　　　　　　　　　　　　&nbsp;&nbsp;Z[] = 1　0 0 2  
将Z[]右移一位, 高位补0　　　　　　　　　&nbsp;&nbsp;Z[] = 0　1 0 0 2  
申请数组X[]记录Z[]的rank的Pos(从大到小)　X[] = 5　2 1 3 4  
　　　　　　　　　　　　　　　　　　　　　&nbsp;&nbsp;&nbsp;&nbsp;(2　1 0 0 0)  
**从大到小排列是将Z[]中数按照 从大到小 依次进入树状数组**  
基于排列后的Z[](2 1 0 0 0)进行遍历, 设立头指针pHead(1~n)和尾指针pTail(pHead~n), 当 *pTail = *pHead 时 pTail ++, 确定出区间 pHead~pTail-1 , 用树状数组维护这段区间(树状数组相当于标记作用)(求 sum<sub>j</sub> >= sum<sub>i</sub> ; **W[] 只是为了具象而设立, 真实为 sum**)：  
　　　　　　　　　　　　　　　　　　　W[] = 0　0 0 0 0  
使 ans += GetSum(X[pHead~pTail-1] - 1)  
**一种类贪心的做法, 因为在Z[]中是降序排列并且标记到的树状数组, 所以在 pHead~pTail-1 区间中 未标记为0 标记为 1, 未标记数一定比标记数小, 就巧妙的求出了 sum<sub>j</sub> >= sum<sub>i</sub>**  
更新树状数组值 ： Add(X[pHead~pTail-1], 1);  
**Z[]中是降序, 在树状数组中对应(当前rank在Z[]中Pos)标记**  
每次 while 后的W[]：  
　　　　　　　　　　　　　　　　　　　 W[] = 0　0 0 0 1  
　　　　　　　　　　　　　　　　　　　 W[] = 0　1 0 0 1
最终 ans=2.  

**将 r - A[]**  
同理上面, 最终ans += 1 ans = 3  

**比较乱, ╮(╯▽╰)╭, 多看几遍就好**  

## 代码

```c++
/*****************************
Description: 预编译, c11平台上
freopen(freopen_s)的安全警告
*****************************/
#define _CRT_SECURE_NO_WARNINGS

#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <algorithm>

using namespace std;

/*****************************
Description: 预编译, 实现跨平台
*****************************/
#ifdef unix
#define LL "%lld"
#else
#define LL "%I64d"
#endif

#define lowbit(x) ((x) & (-(x)))
const int maxn = 500010;

int n = 0, l = 0, r = 0;
int y[maxn] = { 0 }, z[maxn] = { 0 }, w[maxn] = { 0 }, x[maxn] = { 0 };
bool cmp(int a, int b) {
	return z[a] > z[b];
}
/******************************
Description: 树状数组_标记(值++)
******************************/
void Tag(int p) {
	while (p <= n) {
		w[p] ++;
		p += lowbit(p);
	}
}

/**********************************
Description: 树状数组_求逆序对(求和)
**********************************/
int Get(int p) {
	int ans = 0;
	while (p > 0) {
		ans += w[p];
		p -= lowbit(p);
	}
	return ans;
}
/*********************************
Description: 求不满足区间的区间个数
*********************************/
long long solve() {
	//前缀和
	for (int a = 1; a <= n; a++) z[a] += z[a - 1];
	//Z[] 整体后移一位 第一位补0
	for (int a = n + 1; a >= 2; a--) z[a] = z[a - 1];
	z[1] = 0;
	n++;

	for (int a = 1; a <= n; a++) x[a] = a;    //x[] 顺序赋值 x[1]=1 x[2]=2 ... 
	sort(x + 1, x + n + 1, cmp);    //以z[]为关键字排序, x[]表示z[]中每个数的rank(从大到小), 即 Z[X[1]] 为最大数, Z[X[2]] 次之 ...

	memset(w, 0, sizeof(w));
	long long ans = 0;
	int a = 1;
	while (a <= n) {
		int b = a;
		while (b <= n && z[x[b]] == z[x[a]]) b++;    //对于降序的 z[], 当 后面移动数=前面固定数 时, b++ 确定区间
		for (int c = a; c < b; c ++) 
			ans += Get(x[c] - 1);    //x[c]-1 表示前 (当前rank在z[]中pos - 1) 位 获取比当前数大的数的个数
		for (int c = a; c < b; c ++)
			Tag(x[c]);    //树状数组中的 (当前rank在z[]中pos位) ++ 标记
		a = b;    //b指针 -> a指针
	}
	n--;    //恢复原数位
	return ans;
}

/************************
Description: 求最大公因数
************************/
long long gcd(long long a, long long b) {
	if (!b) return a;
	else return gcd(b, a%b);
}

int main() {
	long long ans = 0;

	scanf("%d %d %d", &n, &l, &r);
	for (int a = 1; a <= n; a++) scanf("%d", &y[a]);

	//sum
	for (int a = 1; a <= n; a++) z[a] = y[a] - l;
	ans += solve();

	//sum
	for (int a = 1; a <= n; a++) z[a] = r - y[a];
	ans += solve();

	long long up = ans;    //不满足区间的区间个数
	long long down = (long long)n * (n + 1) / 2;    //分母
	up = down - up;    //分子

	long long g = gcd(up, down);    //最大公因数
	up /= g;    //约分
	down /= g;    //约分
	if (up == 0) printf("0\n");
	else {
		if (down == 1) printf("1\n");
		else printf(LL "/" LL "\n", up, down);
	}

	return 0;
}
```

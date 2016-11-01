---
layout: post
title: "【CODEVS 1222】 信与信封问题"
date: 2016-11-01
tags: [C++, Noip, Bipartite Graph]
categories: [Noip]
bgp: "bg_text_11"
---

**题目链接**：[数字串](http://codevs.cn/problem/1222/)  

## 定义

`匹配` ： 一个边的集合，其中任意两条边都没有公共顶点.  
`最大匹配` ： 一个图所有匹配中，所含匹配边数最多的匹配，称为这个图的最大匹配.  
`完美匹配` ： 如果一个图的某个匹配中，所有的顶点都是匹配点，那么它就是一个完美匹配. **显然，完美匹配一定是最大匹配**  

## 思路

第一眼看到题目很容易就想到是 `二分图` 模型， 但和平常的二分图又不太一样， 题目中说 `第i封信肯定不是装在信封j中` ， 简单的二分图会这样说 `第i封信肯定装在信封j中` ， 这时候就应该换一种思考方式了。  

我们这样考虑 ： 这个二分图最后肯定是一个完美匹配(最大匹配)， 即每封信都有其所对应的信封， 只不过有的信只能放在特定的信封中， 有的信则无所谓。  如果我们特定的信没有放进特定的信封， 最后肯定构不成完美匹配(最大匹配)， 因为这条匹配边是已经确定的， 你取消了它， 肯定要发生矛盾， 不合题意。  

得出结果 ： 开始我们让可能的信与信封相连, 跑一遍 `匈牙利算法` ，如果最后不是完美匹配则直接输出 none ， 之后我们依次删除每个相连边， 如果删除某条相连边后， 最后不是完美匹配， 就可以确定这条边相连的信一定在信封里面.

## 代码

```c++
/*
* Copyright © Eliot
* Date: 2016-11-1
* Author: Eliot
* QQ: 1161142536
* Description: 二分图匹配的灵活运用
*/

#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;

const int Max = 101;

int N = 0;
bool Line[Max][Max] = { false };
bool Is[Max] = {false };
int nEnvelope[Max] = { 0 };

void Init() {
	for (int i = 1; i <= N; i ++) {
		for (int j = 1; j <= N; j ++) {
			Line[i][j] = true;
		}
	}
}

bool Find(int nLetter) {
    for (int i = 1; i <= N; i ++) {
        if (Line[nLetter][i] && !Is[i]) {
			Is[i] = true;
            if (nEnvelope[i] == 0 || Find(nEnvelope[i])) {
                nEnvelope[i] = nLetter;
                return true; 
            } 
		}
    }
    return false;  
}

int Get() {
	int ans = 0;
	for (int i = 1; i <= N; i ++) {
		if (Find(i)) ans ++;
		memset(Is, false, sizeof(Is));
	}
	memset(nEnvelope, 0, sizeof(nEnvelope));
	
	return ans;
}

int a = 0, b = 0;
bool bFlag = false;
int main() {
	scanf("%d", &N);
	
	Init();
	scanf("%d %d", &a, &b);
	while (a != 0 && b != 0) {
		Line[a][b] = false;
		
		scanf("%d %d", &a, &b);
	}
	
	if (Get() != N) {
		cout << "none" << endl;
		return 0;
	}
	
	for (int i = 1; i <= N; i ++) {
		for (int j = 1; j <= N; j ++) {
			if (Line[i][j]) {
				Line[i][j] = false;
				if (Get() != N) {
					cout << i << " " << j << endl;
					bFlag = true;
				}
				Line[i][j] = true;
			}
		}
	}
	
	if (!bFlag) cout << "none" << endl;
	return 0;
} 
```

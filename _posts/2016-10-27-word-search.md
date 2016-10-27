---
layout: post
title: "【搜索】单词接龙"
date: 2016-10-27
desc: "【搜索】单词接龙"
keywords: "C++, Noip, Search"
tags: [C++, Noip, Search]
categories: [C++]
---

题目链接： [单词接龙](http://codevs.cn/problem/1018/) <small>2000年NOIP全国联赛提高组</small>  

很简单的一道搜索题， 但是我交了3遍才AC， 主要是**细节**， 一个细节不注意或者一个环节没有考虑到就会`WA`， 但是**联赛的时候只有一次机会**，所以还是要提高自己一遍AC的能力(太依赖OJ也不好╮(╯▽╰)╭).  
同时还要保持代码的优雅啊~~~  

```c++
/*
* Copyright © Eliot
* Date: 2016-10-27
* Author: Eliot
* QQ: 1161142536
* Description: 单词接龙
*/

#include <iostream>
#include <cstdio>
#include <string>
#include <map>
using namespace std;

int ans = 0;
map<string, int> Map;
int Judge(string strA, string strB) {
	int nLen = min(strA.length(), strB.length());
	for (int i = 1; i < nLen; i ++) {
		string strLast = strA.substr(strA.length() - i, i);
		string strFirst = strB.substr(0, i);
		
		if (strLast == strFirst) {
			return i;
		}
	}
	return -1;
}

int N = 0;
string strWord[21];
void DFS(string str, int nLen) {
	if (str.length() == 1) {
		for (int i = 1; i <= N; i ++) {
			char ch_1 = strWord[i][0], ch_2 = str[0];
			if (ch_1 == ch_2) {
				Map[strWord[i]] ++;
				if (Map[strWord[i]] > 2) {
					Map[strWord[i]] --;
					continue;
				}
				
				nLen += strWord[i].length(); ans = max(ans, nLen);
				DFS(strWord[i], nLen);
				nLen -= strWord[i].length();
				Map[strWord[i]] --;
			}
		}
	}
	else {
		for (int i = 1; i <= N; i ++) {
			if ((strWord[i].find(str) != string::npos && strWord[i] != str) || (str.find(strWord[i]) != string::npos && strWord[i] != str)) continue;
			int nCoincide = Judge(str, strWord[i]);
			if (nCoincide != -1) {
				Map[strWord[i]] ++;
				if (Map[strWord[i]] > 2) {
					Map[strWord[i]] --;
					continue;
				}
				
				nLen += strWord[i].length() - nCoincide; ans = max(ans, nLen);
				DFS(strWord[i], nLen);
				nLen -= strWord[i].length() - nCoincide;
				Map[strWord[i]] --;
			}
		}
	}
}

string str;
int main () {
	scanf("%d", &N);
	for (int i = 1; i <= N; i ++) {
		cin >> strWord[i];
	}
	cin >> str;
	DFS(str, 0);
	
	cout << ans << endl;
	return 0;
}
```

**对题目、代码有疑问的下面留言哦！**

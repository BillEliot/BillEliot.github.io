---
layout: post
title: "【Study】 二分的正确写法"
date: 2016-11-07
tags: [C++, Noip, Binary, Algorithm]
categories: [Noip]
bgp: "bg_text_9"
---

## 二分的正确写法

**整数二分**

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

//注意当l为0时, 输出0, 而非-1
cout << l - 1 << endl;
```

**实数二分**

```c++
double l = 0, r = n;
//精度具体看题意
while (r - l > 0.0001) {
	double nMid = (l + r) / 2.0;
	if (Judge(x, y, nMid)) {
		l = nMid;
	}
	else {
		r = nMid;
	}
}

cout << l << endl;
```

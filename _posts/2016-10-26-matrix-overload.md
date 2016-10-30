---
layout: post
title: "【Study】矩阵乘法 重载运算符"
date: 2016-10-25
desc: "【Study】矩阵乘法 重载运算符"
keywords: "C++, Noip, Matrix, Overload"
tags: [C++, Noip, Matrix, Overload]
categories: [Noip]
bgp: "bg_text_6"
---

*矩阵乘法的计算法则不再赘述，请自行Google。*  
提供模板：  

```c++
/*
* Copyright © Eliot
* Date: 2016-10-26
* Author: Eliot
* Description: 矩阵乘法模板
* QQ: 1161142536
*/

#include <iostream>
#include <cstring>
#include <cstdio>
using namespace std;

#define MAX 201

struct MATRIX{
	long long M[MAX][MAX];
	MATRIX() {
		memset(M, 0, sizeof(M));
	}
}M_1, M_2;

MATRIX Calc(MATRIX A, MATRIX B) {
	MATRIX ANS;
	
	for (int i = 1; i < MAX; i ++) {
		for (int j = 1; j < MAX; j ++) {
			for (int k = 1; k < MAX; k ++) {
				ANS.M[i][j] += A.M[i][k] * B.M[k][j];
			}
		}
	}
	
	return ANS;
}

int x1 = 0, y1 = 0, x2 = 0, y2 = 0;
int main() {
	scanf("%d %d", &x1, &y1);
	for (int i = 1; i <= x1; i ++) {
		for (int j = 1; j <= y1; j ++) {
			scanf("%lld", &M_1.M[i][j]);
		}
	}
	scanf("%d %d", &x2, &y2);
	for (int i = 1; i <= x2; i ++) {
		for (int j = 1; j <= y2; j ++) {
			scanf("%lld", &M_2.M[i][j]);
		}
	}
	
	MATRIX ANS = Calc(M_1, M_2);
	for (int i = 1; i <= x1; i ++) {
		for (int j = 1; j <= y2; j ++) {
			cout << ANS.M[i][j] << " ";
		}
		cout << endl;
	}
	
	return 0;
}
```

## 重载运算符

**有能力则看, 能力不够记住模板即可(因为牵扯到了过多面向对象的知识)**  

### 介绍

C++中预定义的运算符的操作对象只能是基本数据类型。但实际上，对于许多用户自定义类型（例如类），也需要类似的运算操作。这时就必须在C++中重新定义这些运算符，赋予已有运算符新的功能，使它能够用于特定类型执行特定的操作。运算符重载的实质是函数重载，它体现了C++的可扩展性。  

**重载运算符的一般格式：**  

```c++
(friend) <返回值类型> operator<运算符符号> (<参数表>) {
     <函数体>
}
```
operator<运算符符号> 可以看做函数名。  
参数表包含一个隐式的 ```this``` 指针(重载为成员函数)  

**重载运算符需遵循的规则**

* 除了类属关系运算符"."、成员指针运算符".*"、作用域运算符"::"、sizeof运算符和三目运算符"?:"以外，C++中的所有运算符都可以重载。
* 重载运算符限制在C++语言中已有的运算符范围内的允许重载的运算符之中，不能创建新的运算符。
* 运算符重载实质上是函数重载，因此编译程序对运算符重载的选择，遵循函数重载的选择原则。
* 重载之后的运算符不能改变运算符的优先级和结合性，也不能改变运算符操作数的个数及语法结构。
* 运算符重载不能改变该运算符用于内部类型对象的含义。它只能和用户自定义类型的对象一起使用，或者用于用户自定义类型的对象和内部类型的对象混合使用时。
* 运算符重载是针对新类型数据的实际需要对原有运算符进行的适当的改造，重载的功能应当与原有功能相类似，避免没有目的地使用重载运算符。

### 重载成员函数

函数的参数个数比原来的操作数要少一个(后置单目运算符除外), 这是因为成员函数用 `this` 指针隐式地访问了类的一个对象, 它充当了运算符函数左边的操作数.  
* 双目运算符重载为类的成员函数时,函数只显式说明一个参数，该形参是运算符的右操作数  
* 前置单目运算符重载为类的成员函数时，不需要显式说明参数，即函数没有形参  
* 后置单目运算符重载为类的成员函数时，函数要带有一个整型形参  

### 重载友元函数

类的友元函数是定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员。尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数。友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元。

当运算符重载为类的友元函数时，由于没有隐含的this指针，因此操作数的个数没有变化，所有的操作数都必须通过函数的形参进行传递，函数的参数与操作数自左至右一一对应.

重载时在函数前声明 ```friend``` ; ```<<``` ```>>```运算符必须用友元函数才能重载  

### 实例

将上述代码修改为重载运算符(结构体重载、成员函数重载、友元函数重载)  

#### 结构体重载

```c++
/*
* Copyright © Eliot
* Date: 2016-10-26
* Author: Eliot
* Description: 结构体重载 矩阵乘法
* QQ: 1161142536
*/

#include <iostream>
#include <cstring>
#include <cstdio>
using namespace std;

#define MAX 201

struct MATRIX{
	long long M[MAX][MAX];
	MATRIX() { memset(M, 0, sizeof(M)); }
	
	MATRIX operator* (MATRIX& A) {
		MATRIX ANS;
		for (int i = 1; i < MAX; i ++) {
			for (int j = 1; j < MAX; j ++) {
				for (int k = 1; k < MAX; k ++) {
					ANS.M[i][j] += this->M[i][k] * A.M[k][j];
				}
			}
		}
		return ANS;
	}
}M_1, M_2;

int x1 = 0, y1 = 0, x2 = 0, y2 = 0;
int main() {
	scanf("%d %d", &x1, &y1);
	for (int i = 1; i <= x1; i ++) {
		for (int j = 1; j <= y1; j ++) {
			scanf("%lld", &M_1.M[i][j]);
		}
	}
	scanf("%d %d", &x2, &y2);
	for (int i = 1; i <= x2; i ++) {
		for (int j = 1; j <= y2; j ++) {
			scanf("%lld", &M_2.M[i][j]);
		}
	}

	MATRIX ANS = M_1 * M_2;
	for (int i = 1; i <= x1; i ++) {
		for (int j = 1; j <= y2; j ++) {
			cout << ANS.M[i][j] << " ";
		}
		cout << endl;
	}
	
	return 0;
}
```

#### 成员函数重载

```c++
/*
* Copyright © Eliot
* Date: 2016-10-26
* Author: Eliot
* Description: 成员函数重载 矩阵乘法
* QQ: 1161142536
*/

#include <iostream>
#include <cstring>
#include <cstdio>
using namespace std;

#define MAX 201

class MATRIX{
public:
	long long M[MAX][MAX];
	MATRIX() { memset(M, 0, sizeof(M)); }
	~MATRIX() {}
	
public:
	MATRIX operator* (MATRIX& A) {
		MATRIX ANS;
		for (int i = 1; i < MAX; i ++) {
			for (int j = 1; j < MAX; j ++) {
				for (int k = 1; k < MAX; k ++) {
					ANS.M[i][j] += this->M[i][k] * A.M[k][j];
				}
			}
		}
		return ANS;
	}
};

int x1 = 0, y1 = 0, x2 = 0, y2 = 0;
MATRIX M_1, M_2;
int main() {
	scanf("%d %d", &x1, &y1);
	for (int i = 1; i <= x1; i ++) {
		for (int j = 1; j <= y1; j ++) {
			scanf("%lld", &M_1.M[i][j]);
		}
	}
	scanf("%d %d", &x2, &y2);
	for (int i = 1; i <= x2; i ++) {
		for (int j = 1; j <= y2; j ++) {
			scanf("%lld", &M_2.M[i][j]);
		}
	}

	MATRIX ANS = M_1 * M_2;
	for (int i = 1; i <= x1; i ++) {
		for (int j = 1; j <= y2; j ++) {
			cout << ANS.M[i][j] << " ";
		}
		cout << endl;
	}
	
	return 0;
}
```

#### 友元函数重载

```c++
/*
* Copyright © Eliot
* Date: 2016-10-26
* Author: Eliot
* Description: 友元函数重载 矩阵乘法
* QQ: 1161142536
*/

#include <iostream>
#include <cstring>
#include <cstdio>
using namespace std;

#define MAX 201

class MATRIX{
public:
	long long M[MAX][MAX];
	MATRIX() { memset(M, 0, sizeof(M)); }
	~MATRIX() {}
	
public:
	friend MATRIX operator* (MATRIX&, MATRIX&);
};

MATRIX operator* (MATRIX& A, MATRIX& B) {
	MATRIX ANS;
		for (int i = 1; i < MAX; i ++) {
			for (int j = 1; j < MAX; j ++) {
				for (int k = 1; k < MAX; k ++) {
					ANS.M[i][j] += A.M[i][k] * B.M[k][j];
				}
			}
		}
	return ANS;
}

int x1 = 0, y1 = 0, x2 = 0, y2 = 0;
MATRIX M_1, M_2;
int main() {
	scanf("%d %d", &x1, &y1);
	for (int i = 1; i <= x1; i ++) {
		for (int j = 1; j <= y1; j ++) {
			scanf("%lld", &M_1.M[i][j]);
		}
	}
	scanf("%d %d", &x2, &y2);
	for (int i = 1; i <= x2; i ++) {
		for (int j = 1; j <= y2; j ++) {
			scanf("%lld", &M_2.M[i][j]);
		}
	}

	MATRIX ANS = M_1 * M_2;
	for (int i = 1; i <= x1; i ++) {
		for (int j = 1; j <= y2; j ++) {
			cout << ANS.M[i][j] << " ";
		}
		cout << endl;
	}
	
	return 0;
}
```

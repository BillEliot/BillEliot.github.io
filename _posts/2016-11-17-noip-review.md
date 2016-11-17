---
layout: post
title: "【终章】 NOIP2016复习导论"
date: 2016-11-17
tags: [C++, Noip, Final, Review]
categories: [Noip]
bgp: "bg_text_26"
---

# 目录

* DFS 优化
* BFS 优化
* 数论
　+ 错排通项
　+ 费马小定理
　+ 逆元
　+ 欧拉函数&欧拉定理
　+ 求解欧拉函数
　+ 扩展欧几里得
　+ GCD&LCM
　+ 素数判断&素数筛
　+ 快速幂
　+ 同余定理
* 图论
　+ 次短路
　+ 拓扑排序

<hr>

### DFS优化

* `最优化剪枝`：求最优值时，当前的状态无论如何不可能比最优值更优，则退出  
* `可行性剪枝`：提前判断该状态是否能得到可行解，如不能则退出  
* `记忆化搜索`：对于已经搜索过的状态直接退出  
* `改变搜索顺序`：对于看起来希望更大的决策先进行搜索  
* 改写成 `IDA*` 算法  
* 卡时(联赛中禁止使用meml掐时)  

<hr>

### BFS优化

* 判重优化：`hash`, `二叉排序树`
* `双向广搜`或`启发式搜索`
* 改写 A*算法
* 二分优化

<hr>

## 数论

### 错排通项

`定义`： 一个有n个元素的排列，若排列中所有的元素都不在自己原来的位置上，那么这样的排列就称为原排列的一个错排。  
`经典问题`：  

* 写信时将n封信装到n个不同的信封里，有多少种全部装错信封的情况？
* 四人各写一张贺年卡互相赠送，有多少种赠送方法？(自己写的贺年卡不能送给自己)  
`错排公式`:  

* D[1]=0    D[2]=1
* D[n] =(n - 1)(D[n - 2] + D[n - 1])
* D(n) = n!(1/0! - 1/1! + 1/2! - 1/3! ...... + ((-1)<sup>n</sup>) * (1 / n!))

当n很大时计算就不方便, 一个供参考的简化后的公式是  
D(n) = [n! / e + 0.5] (e是自然对数的底, [x]为x的整数部分)  

### 费马小定理

设a是一个整数, p是一个素数, 则 a<sup>p</sup> ≡ a (mod p)  
若a不是p的倍数, 该定理也可以写成 a<sup>p-1</sup> ≡ 1 (mod p)  

### 逆元

根据`费马小定理`： 设p是质数, 且 a, p互质(GCD(a, p) = 1), 则 a<sup>p-1</sup> ≡ 1 (mod p)  
`逆元`：若 a<sup>b</sup> ≡ 1 (mod p), 则在 mod p 意义下, a, b互为逆元  

所以若p是质数, 且GCD(a, p) = 1, 则a的逆元是 a<sup>p-2</sup>  
`逆元作用`：mod 意义下进行的除法, 乘a的逆元等同于除以a.  

### 欧拉函数

`欧拉函数`: 对正整数n, `欧拉函数`是指小于或等于n的正整数中与n互质的数的数目  
`欧拉定理`: 若 p, a 为正整数, 且 p, a 互质(GCD(a, p) = 1), 则 a<sup>φ(p)</sup> ≡ 1(mod p) (φ(p)欧拉函数)  
可以看出, 当 p 为质数时, φ(p) = p - 1(1~(p-1)均与p互质), 则 a<sup>p-1</sup> ≡ 1(mod p), 就是`费马小定理`, 费马小定理其实是欧拉定理的一个子集.  

#### 求解欧拉函数

`公式`： φ(x) = x * (1 - 1/p1) * (1 - 1 / p2) * (1 - 1 / p3) * (1 - 1 / p4)...(1 - 1 / pn), 其中p1,  p2...pn为x的所有质因数, x是不为0的整数.(φ(1)=1)  

##### 求解单个欧拉函数

```c++
int Euler(int n) {
     int res = n, a = n;
     for (int i = 2; i * i <= a; i++) {
         if (a % i == 0) {
             //trick 先进行除法防止中间数据溢出
             res = res / i * (i - 1);
             while (a % i == 0) a /= i;
         }
     }
     if(a > 1) res = res / a * (a - 1);
     return res;
}
```

##### 求解多个欧拉函数 <small>比如求前n个数的欧拉函数的和</small>

```c++
const int maxlen = 1000010;
long long eul[maxlen] = { 0 };
int pri[maxlen] = { 0 }, f[maxlen] = { 0 }, isprim[maxlen] = { 0 }, p = 0;
inline void get_prim() {
    memset(isprim, 1, sizeof(isprim));
    for (int i = 2; i < maxlen; i++) {
        if (isprim[i]) pri[p++] = i;
        for(int j = 0; j < p && i * pri[j] < maxlen; j++) {
            int k = pri[j] * i;
            isprim[k] = 0;
            f[k] = pri[j];
            if (i % pri[j] == 0) break;
        }
    }
}
inline void get_eul() {
    for (int i = 2; i < maxlen; i++)
        if (isprim[i]) {
            eul[i] = i - 1;
        }
        else {
            int k = i / f[i];
            if (k % f[i] == 0) 
                eul[i] = eul[k] * f[i];
            else 
                eul[i] = eul[k] * (f[i] - 1);
        }
    for(int i = 3; i < maxlen; i++)
        eul[i] += eul[i - 1];
}

int main() {
    int m = 0;
    get_prim();
    get_eul();
    while (scanf("%d", &m))
        printf("%lld\n", eul[m] + 1);
}
```

### 扩展欧几里得

### GCD&LCM

```c++
int GCD(int m, int n) {
	while (m % n != 0) {
		int temp = m;
		m = n;
		n = temp % n;
	}
	return n;
}
```

设两个数 m, n 有公式 `GCD(m, n) * LCM(m, n) = m * n`  

### 素数

#### 判定单个素数

```c++
bool Judge(int nNum) {
	if (nNum == 1) return false;
	for (int i = 2; i <= sqrt(nNum); i++) {
		if (nNum % i == 0) return false;
	}
	return true;
}
```

#### 线性素数筛

```c++
#include <iostream>
using namespace std;
const long N = 200000;
long prime[N] = { 0 }, num_prime = 0;
int isNotPrime[N] = { 1 };
int main() {
        for (long i = 2; i < N; i++) {
        if (!isNotPrime[i])
        	prime[num_prime++] = i;
        for (long j = 0 ; j < num_prime && i * prime[j] <  N; j++) {
                isNotPrime[i * prime[j]] = 1;
            if (!(i % prime[j]))
                break;
        }
    }
    for (int i = 0; i < num_prime; i++) printf("%d ", prime[i]);
    return 0;
}
```

### 快速幂

```c++
long long PowerMod(int m, int n, int p) {
	long long ans = 1;
	while (n > 0) {
		if (n & 1) {
			ans *= m;
			ans %= p;
		}
		m *= m;
		n >>= 1;
		m %= p;
	}
	return ans;
}
```

### 同余定理

<hr>

## 图论

### 次短路

`双向最短路` ：  
1. 超级源点 dijkstra(SPFA)  
2. 反向建图, 超级汇点dijkstra(SPFA)  
3. for all edges (u, v, w)  
　　　if ((源到u的距离) + (汇到v的距离) + w) 不是最短路且比当前次短路更优  
　　　　更新次短路
`删边法` :  
每次删最短路上的一条边, 用 `Dijkstra+堆` 求单源最短路径, 找最优.  

### 拓扑排序

`定义`： 对`有向无环图`的顶点的一种排序, 它使得如果存在一条从顶点A到顶点B的路径, 那么在排序中B出现在A的后面.  
`限定条件`: 

* 有向无环图(DAG)
* 每个顶点出现且只出现一次  
* 若A在序列中排在B的前面, 则在图中不存在从B到A的路径  

`求解思想`：  

* 从 DAG 中选择一个入度为0(没有前驱)的顶点输出  
* 从 DAG 中删除该顶点以及所有以它为起点的有向边  
* 重复以上两个过程直至当前的 DAG  为空或当前图中不存在无前驱的顶点为止(有向图中存在环).  

`应用`：  
拓扑排序通常用来 "排序" 具有依赖关系的任务.  
比如，如果用一个 DAG 来表示一个工程，其中每个顶点表示工程中的一个任务，用有向边表示在做任务 B 之前必须先完成任务 A。故在这个工程中，任意两个任务要么具有确定的先后关系，要么是没有关系，绝对不存在互相矛盾的关系(即环路).  

```c++
#include <iostream>
#include <list>
#include <queue>
using namespace std;

class Graph {
	int V;             // 顶点个数
	list<int> *adj;    // 邻接表
	queue<int> q;      // 维护一个入度为0的顶点的集合
	int* indegree;     // 记录每个顶点的入度
public:
	Graph(int V);                   // 构造函数
	~Graph();                       // 析构函数
	void addEdge(int v, int w);     // 添加边
	bool topological_sort();        // 拓扑排序
};

Graph::Graph(int V) {
	this->V = V;
	adj = new list<int>[V];
	indegree = new int[V];           
	for(int i = 0; i < V; i++)       // 入度全部初始化为0
		indegree[i] = 0;
}
Graph::~Graph() {
	delete [] adj;
	delete [] indegree;
}

void Graph::addEdge(int v, int w) {
	adj[v].push_back(w);
	indegree[w] ++;
}

bool Graph::topological_sort() {
	for (int i = 0; i < V; i++)
		if(indegree[i] == 0)
			q.push(i);         // 将所有入度为0的顶点入队

	int count = 0;             // 计数，记录当前已经输出的顶点数 
	while (!q.empty()) {
		int v = q.front();      // 从队列中取出一个顶点
		q.pop();

		cout << v << " ";      // 输出该顶点
		count ++;
		// 将所有v指向的顶点的入度减1，并将入度减为0的顶点入栈
		list<int>::iterator beg;
		for (beg = adj[v].begin(); beg != adj[v].end(); beg++)
			if ((--indegree[*beg]) == 0)
				q.push(*beg);   // 若入度为0，则入队
	}

	if(count < V)
		return false;           // 没有输出全部顶点, 有向图中有回路
	else
		return true;            // 拓扑排序成功
}
```


---
layout: post
title: "NOIP_Algo类文档"
date: 2016-08-30
desc: "NOIP_Algo类文档"
keywords: "C++,class"
categories: [database]
---

参加竞赛的时候，将NOIP主要的算法做了一个总结，便有了现在的Class:NOIP_Algo，因为特殊性，类中大部分函数需要重载，  
本类只提供参考，如果大家发现错误的话也希望和我联系。

```
主类：CNOIP_ALGO
    CNOIP_ALGO派生类:
    CHighPrecision
    CMathTheory
    CGraphTheory
    CDataStruct
      CDisjointSet
      CTreeArray
      SegmentTree
    CBinaryConver
```

### CHighPrecision
      Function:
      string HighAdd(string strA,string strB); 高精度加法
      string HighDec(string strA,string strB); 高精度减法
      string HighMul(string strA,string strB); 高精度乘法
	      
### CMathTheory
       Function:
       bool IsPrime(int nNum);                  素数判定
       int* PrimeSieve(int MaxNum = 1000);      素数筛法
       int QuickMod(int m,int n,int nMod);      快速幂
       int GetFactorial(int nNum);              求阶乘
       int A(int a,int b);                      排列
       int C(int a,int b);                      组合
          
### CGraphTheory
        Function:
        int* SPFA(int nStart);                  SPFA
	int* Dijkstra(int nStart);              迪杰斯特拉
	int Kruskal(KRUSKAL* pS_Kruskal,int nMax_Line);   克鲁斯卡尔
          
### CDisjointSet
        Function:
        void Init();                             初始化
	int Find(int nPos);                      查找父亲
	void Union(int nFather_A,int nFather_B); 合并集合
          
### CTreeArray
        int Lowbit(int N);                       Lowbit
	void Add(int nPos,int nOffset);          加偏移量
	int Sum(int N);                          求区间和
          
### CBinaryConversion
        int BinaryToTen(string strNum,int nCur_Binary);   任意进制转10进制
	string TenToBinary(int nNum,int nTo_Binary);      10进制转任意进制
	string BinaryToBinary(string strNum,int nCur_Binary,int nTo_Binary); 任意进制转任意进制

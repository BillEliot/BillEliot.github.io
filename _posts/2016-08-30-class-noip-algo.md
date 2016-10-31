---
layout: post
title: "NOIP_Algo类文档"
date: 2016-08-30
desc: "NOIP_Algo类文档"
keywords: "C++,class"
categories: [Noip]
---

参加竞赛的时候，将NOIP主要的算法做了一个总结，便有了现在的Class:NOIP_Algo，因为特殊性，类中大部分函数需要重载，  
本类只提供参考，如果大家发现错误的话也希望和我联系。

主类：CNOIP_ALGO  
    CNOIP_ALGO派生类:
    
* [CHighPrecision](#HP)
* [CMathTheory](#MT)
* [CGraphTheory](#GT)
* [CDataStruct]
  * [CDisjointSet](#DSe)
  * [CTreeArray](#TA)
  * [SegmentTree](#ST)
* [CBinaryConver](#BC)

<h3 id = "HP">CHighPrecision</h3>

``` c++
  //Function:
      string HighAdd(string strA,string strB); //高精度加法
      string HighDec(string strA,string strB); //高精度减法
      string HighMul(string strA,string strB); //高精度乘法
```

<h3 id = "MT">CMathTheory</h3>

``` c++      
  //Function:
      bool IsPrime(int nNum);                  //素数判定
      int* PrimeSieve(int MaxNum = 1000);      //素数筛法
      int QuickMod(int m,int n,int nMod);      //快速幂
      int GetFactorial(int nNum);              //求阶乘
      int A(int a,int b);                      //排列
      int C(int a,int b);                      //组合
```          

<h3 id = "GT">CGraphTheory</h3>

``` c++
  //Function:
      int* SPFA(int nStart);                  //SPFA
      int* Dijkstra(int nStart);              //迪杰斯特拉
      int Kruskal(KRUSKAL* pS_Kruskal,int nMax_Line);   //克鲁斯卡尔
```          

<h3 id = "DSe">CDisjointSet</h3>
      
``` c++  
  //Function:
      void Init();                             //初始化
      int Find(int nPos);                      //查找父亲
      void Union(int nFather_A,int nFather_B); //合并集合
```          

<h3 id = "TA">CTreeArray</h3>
      
``` c++
  //Function:
      int Lowbit(int N);                       //Lowbit
      void Add(int nPos,int nOffset);          //加偏移量
      int Sum(int N);                          //求区间和
```          

<h3 id = "BC">CBinaryConversion</h3>
      
``` c++
  //Function:
      int BinaryToTen(string strNum,int nCur_Binary);   //任意进制转10进制
      string TenToBinary(int nNum,int nTo_Binary);      //10进制转任意进制
      string BinaryToBinary(string strNum,int nCur_Binary,int nTo_Binary); //任意进制转任意进制
```

## 类实现

```c++
/*****************************************
CopyRight © 2016 ~ ∞
Author:WJZ(Eliot)
Date：2016.8.12
*****************************************/
#include "Algo.h"

/*****************************************
Class:HighPrecision
Description:高精度运算类
               高精度加法
               高精度减法
               高精度乘法
               高精度除法
               高精度取模
*****************************************/
string CHighPrecision::HighAdd(string strA, string strB){
	Init();

	nLenA = strA.length();
	nLenB = strB.length();
	for(nPos = 0; nPos < nLenA; nPos ++) pA[nLenA - nPos - 1] = strA[nPos] - 48;
	for(nPos = 0; nPos < nLenB; nPos ++) pB[nLenB - nPos - 1] = strB[nPos] - 48;
	nLenAns = max(nLenA, nLenB);
	for(nPos = 0; nPos < nLenAns; nPos ++) pAns[nPos] = pA[nPos] + pB[nPos];
	for(nPos = 0; nPos < nLenAns; nPos ++){
		pAns[nPos + 1] += pAns[nPos] / 10;
		pAns[nPos] %= 10;
	}
	if(pAns[nLenAns] > 0) nLenAns ++;
	for(nPos = nLenAns - 1;nPos >= 0;nPos --){
		strAns += pAns[nPos] + 48;
	}

	return strAns;
}
string CHighPrecision::HighDec(string strA, string strB){
	Init();

	if((strA.length() < strB.length()) || (strA.length() == strB.length() && strA < strB)){
		string temp = strA;
		strA = strB;
		strB = temp;
		strAns += "-";
	}
	nLenA = strA.length(); 
	nLenB = strB.length();
	for(nPos = 0; nPos < nLenA; nPos ++) pA[nLenA - nPos] = strA[nPos] - 48;
	for(nPos = 0; nPos < nLenB; nPos ++) pB[nLenB - nPos] = strB[nPos] - 48;

	nPos = 0;
	while(nPos <= nLenA){
		if(pA[nPos] < pB[nPos]){
			pA[nPos] += 10;
			pA[nPos + 1] --;
		} 
		pAns[nPos] = pA[nPos] - pB[nPos]; 
		nPos ++; 
	}
	nLenAns = nPos;
	while((pAns[nLenAns] == 0) && (nLenAns > 1)) nLenAns --; 
	for(nPos = nLenAns;nPos >= 1;nPos --){
		strAns += pAns[nPos] + 48;
	}

	return strAns;
}
string CHighPrecision::HighMul(string strA, string strB){
	Init();

	nLenA = strA.length(); 
	nLenB = strB.length(); 
	for(nPos = 0; nPos < nLenA; nPos ++) pA[nLenA - nPos] = strA[nPos] - 48;
	for(nPos = 0; nPos < nLenB; nPos ++) pB[nLenB - nPos] = strB[nPos] - 48;

	int x = 0;
	for(nPos = 1; nPos <= nLenA; nPos ++){
		x = 0;
		for(int i = 1; i <= nLenB; i ++){
			pAns[nPos + i - 1] = pA[nPos] * pB[i] + x + pAns[nPos + i - 1];
			x = pAns[nPos + i - 1] / 10;
			pAns[nPos + i - 1] %= 10;
		}
		pAns[nPos + nLenB] = x;
	}
	nLenAns = nLenA + nLenB;
	while(pAns[nLenAns] == 0 && nLenAns > 1) nLenAns --;
	for(nPos = nLenAns;nPos >= 1;nPos --){
		strAns += pAns[nPos] + 48;
	}

	return strAns;
}
/*****************************************
Class:CMathTheory
Description:数论类
               素数判断 -> 判断所给数字是否为素数
               素数筛 -> 筛选出2~N之间的所有素数
               快速幂 -> 快速计算 m ^ n % k
               求阶乘 -> 计算 N!
               排列 -> 计算A(m,n)
               组合 -> 计算C(m,n)
*****************************************/
 bool CMathTheory::IsPrime(int nNum){
	 for(int i = 2; i <= sqrt(nNum); i ++){
		 if(nNum % i == 0) return false;
	 }
	 return true;
 }
int* CMathTheory::PrimeSieve(int nMaxNum){
	 int nCount = 0;
	 bool* Is = (bool*)malloc(sizeof(Is) * nMaxNum);
	 pPrime = new int[nMaxNum];

	 memset(Is, false, sizeof(Is) * nMaxNum);
	 memset(pPrime, 0, sizeof(nMaxNum) * nMaxNum);

	 for(int i = 2; i < nMaxNum; i ++){
		 if (!Is[i]) pPrime[nCount ++] = i;
		 for (int j = 0;j < nCount && pPrime[j] * i < nMaxNum;j ++){
			 Is[pPrime[j] * i] = true;
			 if(i % pPrime[j] == 0) break;
		 }
	 }

	 free(Is);
	 return pPrime;
 }
int CMathTheory::QuickMod(int m, int n, int nMod){
	int nAns = 1;
	while(n > 0){
		if(n & 1){
			nAns *= m;
			nAns %= nMod;
		}
		m *= m;
		n >>= 1; 

		m %= nMod;
	}

	return nAns;
}
int CMathTheory::GetFactorial(int nNum){
	int nAns = 1;
	for(int i = 1; i <= nNum; i ++){
		nAns *= i;
	}
	return nAns;
}
//A(a,b)
int CMathTheory::A(int a, int b){
	return GetFactorial(a) / GetFactorial(a - b);
}
//C(a,b)
int CMathTheory::C(int a, int b){
	return GetFactorial(a) / (GetFactorial(a - b) * GetFactorial(b));
}
/*****************************************
Class:CGraphTheory
Description:图论类
               Read(OverWrite)
               SPFA(SLF优化) -> 求单源最短路径
               Dijkstra(优先队列堆优化) -> 求单源最短路径
               Kruskal -> 求最小生成树(MST)
*****************************************/
int* CGraphTheory::SPFA(int nStart){
	deque<int> Q;
	int* pDis = new int[nMaxArray];
	bool* pIsStack = new bool[nMaxArray];
	memset(pDis, 127 / 3, sizeof(pDis) * nMaxArray);
	memset(pIsStack, false, sizeof(pIsStack) * nMaxArray);
	
	Q.push_front(nStart);
	pIsStack[nStart] = true;
	pDis[nStart] = 0;
	while(!Q.empty()){
		int temp = Q.front();
		Q.pop_front();
		pIsStack[temp] = false;

		for(UINT i = 0; i < pArray[temp].size(); i ++){
			int nNextPoint = pArray[temp][i];
			if(pDis[nNextPoint] > pDis[temp] + pWeight[temp][i]){
				pDis[nNextPoint] = pDis[temp] + pWeight[temp][i];
				if(!pIsStack[nNextPoint]){
					pIsStack[nNextPoint] = true;
					//SLF优化(新值加入队列时要比较dis值，dis小于队首元素的dis值就加到队首，否则加入队尾)
					if(pDis[nNextPoint] < pDis[Q.front()] && Q.size() != 0){
						Q.push_front(nNextPoint);
					}else{
						Q.push_back(nNextPoint);
					}
				}
			}
		}
	}

	delete[] pDis;
	delete[] pIsStack;
	return pDis;
}
int* CGraphTheory::Dijkstra(int nStart){
	


	return 0;
}
/****************************
Description:You need to use
S_Kruskal[MAX_ARRAY] to
save data when OverWriting
Function Read()
****************************/
int CGraphTheory::Kruskal(KRUSKAL* pS_Kruskal, int nMax_Line){
	int nAns = 0;
	
	//定义并查集
	CDisjointSet *pDisjointSet = new CDisjointSet;
	pDisjointSet->Init();

	sort(pS_Kruskal + 1, pS_Kruskal + nMax_Line + 1, comp);
	for(int i = 1; i <= nMax_Line; i ++){
		int nFather_A = pDisjointSet->Find(pS_Kruskal[i].x);
		int nFather_B = pDisjointSet->Find(pS_Kruskal[i].y);
		if(pDisjointSet->pFather[nFather_A] != pDisjointSet->pFather[nFather_B]){
			pDisjointSet->Union(nFather_A, nFather_B);
			nAns += pS_Kruskal->c;
		}
	}

	delete pDisjointSet;
	return nAns;
}
/*****************************************
Class:CDisjointSet
Description:并查集类(路径压缩)
               Init -> 初始化
               Find -> 向上查找父亲
               Union -> 合并集合
*****************************************/
void CDisjointSet::Init(){
	for(int i = 0; i <= _nMaxArray; i ++){
		pFather[i] = i;
	}
}
int CDisjointSet::Find(int nPos){
	if(pFather[nPos] != nPos){
		pFather[nPos] = Find(pFather[nPos]);
	}
	return pFather[nPos];
}
void CDisjointSet::Union(int nFather_A, int nFather_B){
	pFather[nFather_B] = nFather_A;
}
/*****************************************
Class:CTreeArray : public CDataStruct
Description:树状数组类
               Lowbit
               Add -> 修改偏移值(+n or -n)
               Sum -> 求1~N区间和
*****************************************/
int CTreeArray::Lowbit(int N){
	return N&(-N);
}
void CTreeArray::Add(int nPos, int nOffset){
	while(nPos < nRight_Section){
		pSum[nPos] += nOffset;
		nPos += Lowbit(nPos);
	}
}
int CTreeArray::Sum(int N){
	int sum = 0;
	while(N > 0){
		sum += pSum[N];
		N -= Lowbit(N);
	}

	return sum;
}
/*****************************************
Class:CBinaryConversion
Description:进制转换类(最高到16进制)
               任意进制转10进制
               10进制转任意进制
               任意进制转任意进制m
*****************************************/
int CBinaryConver::BinaryToTen(string strNum, int nCur_Binary){
	int nLen = strNum.length();
	int nResult = 0;

	for(int i = 1; i <= nLen; i ++){
		nResult += ConverToInt(strNum[i - 1]) * pow(nCur_Binary,nLen - i);
	}

	return nResult;
}
string CBinaryConver::TenToBinary(int nNum, int nTo_Binary){
	ostringstream strNum,strRetn;

	//Init
	strNum.str("");strRetn.str("");

	while(nNum > 0){
		switch(int N = nNum % nTo_Binary){
		case 10:
			strNum << 'A';
			break;
		case 11:
			strNum << 'B';
			break;
		case 12:
			strNum << 'C';
			break;
		case 13:
			strNum << 'D';
			break;
		case 14:
			strNum << 'E';
			break;
		case 15:
			strNum << 'F';
			break;
		default:
			strNum << N;
		}
		nNum /= nTo_Binary;
	}
	for(int i = strNum.str().length() - 1; i >= 0; i --){
		strRetn << strNum.str()[i];
	}

	return strRetn.str();
}
string CBinaryConver::BinaryToBinary(string strNum, int nCur_Binary, int nTo_Binary){
	//传入进制转10进制
	int nTen = BinaryToTen(strNum, nCur_Binary);
	//10进制转目标进制
	return TenToBinary(nTen, nTo_Binary);
}
```

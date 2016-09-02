---
layout: post
title: "程序员面试题"
date: 2016-09-01
desc: "程序员面试题"
keywords: "Interview,programmer"
categories: [Life]
tags: [Interview,programmer]
---

* 题目一

**Q：**我们可以用*static*修饰一个类的成员函数，也可以用*const*修饰类的成员函数(*指写在函数最后表示不能修改成员变量*)。请问：能不能同时用*static*和*const*修饰类的成员函数。
**A：**答案是不可以。C++编译器在实现*const*成员函数的时候为了确保该函数不会修改类的实例状态，会在函数中添加一个隐式的参数const this * 。但当一个成员函数为static时，该函数没有this指针，此时*static*和*const*冲突。  

* 题目二

**Q:**运行以下代码，输出结果是什么？  

``` c++
class A{
};
class B{
public:
    B() {}
    ~B() {}
};
class C{
public:
    C() {}
    virtual ~C() {}
};

int _tmain(int argc,_TCHAR* argv[]){
    printf("%d %d %d\n",sizeof(A),sizeof(B),sizeof(C));
    return 0;
}
```

**A：**答案是：1 1 4。 *class A* 是一个空类，它的实例不包含任何信息，按理说应该是 sizeof(A) = 0，但当我们声明该类型的时候，它必须在内存中占有一定的空间，否则无法使用该实例。至于占多少内存由编译器决定(如VS2008中空类占用 1byte 空间)。  
*class B* 在 *class A* 的基础上增加了构造函数和析构函数，由于构造函数和析构函数的调用与类型的实例无关(调用它们只需要知道函数地址即可)，在它的实例中不需要增加任何信息，所以 sizeof(A) = sizeof(B)。  
*class C* 在 *class B* 的基础上将析构函数标注为虚函数，C++编译器一发现类型中有虚函数，就会为该类型生成虚函数表，并在该类型的实例中添加一个指向虚函数表的指针。32位机器上，一个指针占用4byte空间，所以 sizeof(C) = 4。  
(*详细请参见我另一篇博文：[C++类的深入 ](https://billeliot.github.io/database/2016/08/31/C-class.html)*)

* 题目三

**Q：**运行以下代码，输出结果是什么？  

``` c++
class A{
private:
    int m_Value;
public:
    A(int nValue){
        m_Value = nValue;
    }
    
    void Print1(){
        printf("Hello World");
    }
    void Print2(){
        printf("%d",m_Value);
    }
};

int _tmain(int argc,_TCHAR* argv){
    A* pA = NULL;
    
    pA->Print1();
    pA->Print2();
    
    return 0;
}
```

**A:**答案是*Print1*调用正常，打印出“Hello World”，运行*Print2*时程序崩溃。调用*Print1*时并不需要知道pA地址，因为*Print1*的函数地址是固定的，编译器会给*Print1*传入一个this指针，该指针为NULL，但在*Print1*中this指针并没有用到，只要程序运行时没有访问无效内存就不会出错。运行*Print2*时，需要this指针才会得到m_Value值，但此时this->NULL，因此程序崩溃。  

* 题目四

**Q：**运行以下代码，输出结果是什么？  

``` c++
class A{
private:
    int m_Value;
public:
    A(int nValue){
        m_Value = nValue;
    }
    
    void Print1(){
        printf("Hello World");
    }
    virtual void Print2(){
        printf("Hello World");
    }
};

int _tmain(int argc,_TCHAR* argv){
    A* pA = NULL;
    
    pA->Print1();
    pA->Print2();
    
    return 0;
}
```

**A:**答案同*题目三*。解释Print2崩溃原因：*Print2*是虚函数，C++调用虚函数的时候，需根据实例中虚函数表指针得到虚函数表地址，再从虚函数表里面获取函数地址，这一步需要访问this指针，而this->NULL，因此程序崩溃。  

## 十个要面对的问题

1.你最喜欢的编程语言是什么？你讨厌哪些编程语言？为什么？
 2.如果让你在自己最常用的编程语言上面添加功能，你希望是什么功能？

3.说一个你曾经参与过的项目，在这过程中经历了哪些困难，最后如何克服？

4.你有没有干过什么事情最后却铩羽而归？

5.在某个休息天，突然有同事打电话来要你快速回复有关于你最近写的代码片段的问题，你会不会觉得生气烦躁？

 6.你被要求去搞定一堆艰巨的代码，但是你却不知道它是如何工作的，没有文档也没有测试，你会怎么做？

7.在Zelda系列中你最喜欢什么游戏？你还喜欢哪些？你是否曾想过如果是你先开发的minecraft，那会怎么样？

8.你喜欢什么网站？

9.你会推荐什么书作为必读？

10.最后一个但并非是最不重要的，请解释以下名词：DRY、SOLID、YAGNI、乐观锁与悲观锁）、MVC与MVVM(可自行添加)
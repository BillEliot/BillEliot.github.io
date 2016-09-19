---
layout: post
title: "【翻译】Basic Security Testing with Kali Linux (2014) —— 【五】"
desc: "【翻译】Basic Security Testing with Kali Linux (2014) —— 【五】"
date: 2016-09-20
keywords: [Linux,Translate]
tags: [Linux,Translate]
categories: [Linux]
---

# 第十章 —— Windows Privilege Escalation by Bypassing UAC

## 介绍

Windows上的Admin账户有很大的权限，但是也有一些事情是Admin不能执行的。Linux上的Root账户是超级用户，Windows上的System账户是超级账户。  
User Access Control(UAC)似乎在Windows上很烦人，因此很多用户选择将其关闭。UAC在Windows 7上工作的很好，使用它的最低级别的安全设置就能阻止很多恶意攻击。  
尽管我们使用Metasploit取得了一个Admin账户，UAC也能阻止我们做一些事情，像获取密码哈希值。但是Metasploit有一个绕过UAC的模块使我们去取得System账户。  

## 绕过 UAC

首先将会话放置到后台。  
![alt text](/../static/img/blog/BasicSecurity_5/0.png)  
使用 bypassuac exploit  
![alt text](/../static/img/blog/BasicSecurity_5/1.png)  
使用 show options 查看需设置的变量  
![alt text](/../static/img/blog/BasicSecurity_5/2.png)  
我们只需用 set session ID 设置活跃的会话  
![alt text](/../static/img/blog/BasicSecurity_5/3.png)  
最后，使用 exploit 运行 bypassuac  
![alt text](/../static/img/blog/BasicSecurity_5/4.png)  
非常好，你可以看到用户事实上是administratiors组的一员，UAC bypass开始工作了并且一个新的会话被创建了。  
现在我们再次尝试 getuid，可以看到他仍然显示 user Fred，但是我们执行 getsystem，再次执行 getuid，事实上我们已经获取了System权限。  
![alt text](/../static/img/blog/BasicSecurity_5/5.png)  
![alt text](/../static/img/blog/BasicSecurity_5/6.png)  

现在，如果我们想dump系统的密码哈希，使用脚本 run post/windows/gather/hashdump  
![alt text](/../static/img/blog/BasicSecurity_5/7.png)  
![alt text](/../static/img/blog/BasicSecurity_5/8.png)  

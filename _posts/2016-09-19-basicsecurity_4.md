---
layout: post
title: "【翻译】Basic Security Testing with Kali Linux (2014) —— 【四】"
desc: "【翻译】Basic Security Testing with Kali Linux (2014) —— 【四】"
date: 2016-09-19
keywords: [Linux,Translate]
tags: [Linux,Translate]
categories: [Linux]
---

# 第九章 —— Windows AV Bypass with Veil

## 介绍

很多人认为如果他们运行杀毒软件或者防火墙，他们就可以远离黑客的攻击。但事实并不是如此。使用 *Veil* 创建的远程Shell模块一般可以免杀很多杀毒软件。  
很多杀毒软件的工作原理是特征码或签名匹配。如果一个程序像恶意软件那样运行着，就会被“杀”。如果一个恶意软件有一个杀毒软件从未见过的签名，大部分情况下杀毒软件会认为它是安全的并且没有威胁。  
如果你可以改变或者掩盖恶意软件或者远程Shell的签名，杀毒软件很可能会放行，这样黑客就获得了一个远程连接。  
Veil，一款新的payload生成器，它生成一个标准的Metasploit payload，并有很大几率免杀杀毒软件。  

## 安装 Veil

键入 apt-get update && apt-get install veil-evasion 安装veil  
![alt text](/../static/img/blog/BasicSecurity_4/0.png)  

## 使用 Veil

使用 list 查看可用payload：  
![alt text](/../static/img/blog/BasicSecurity_4/1.png)  
使用 use ID 来使用payload，本教程中将使用 powershell/VirtualAlloc payload：  
![alt text](/../static/img/blog/BasicSecurity_4/2.png)  
我们将使用默认值，因此键入 generate，这样你就可以选择使用Metasploit的标准msvenom，或者你自己的。我们将使用默认的，即msvenom。  
键入 1 选择  
![alt text](/../static/img/blog/BasicSecurity_4/3.png)  
下一步将选择Shell payload的类型，我们将使用默认，即reverse_TCP。这意味着远程主机将进行回连。  
键入 enter 选择默认值  
![alt text](/../static/img/blog/BasicSecurity_4/4.png)  
键入本机 IP  
![alt text](/../static/img/blog/BasicSecurity_4/5.png)  
键入使用的端口  
![alt text](/../static/img/blog/BasicSecurity_4/6.png)  
键入创建的文件名  
![alt text](/../static/img/blog/BasicSecurity_4/7.png)  
至此，你希望目标主机打开这个文件，因此社会工程学是必要的。  
现在，我们将开启服务来监听连接。  

## 获取远程Shell

我们将使用Metasploit创建。  
按如下步骤创建：  
![alt text](/../static/img/blog/BasicSecurity_4/8.png)  
之后在目标主机上打开后门文件。  

![alt text](/../static/img/blog/BasicSecurity_4/9.png)  
成功连接！  

---
layout: post
title: "【翻译】Basic Security Testing with Kali Linux (2014) —— 【六】"
desc: "【翻译】Basic Security Testing with Kali Linux (2014) —— 【六】"
date: 2016-09-20
keywords: [Linux,Translate]
tags: [Linux,Translate]
categories: [Linux]
---

# 第十一章 —— 数据包拦截和中间人攻击

## 介绍

另一种我们可以利用的高级技术是监视和捕获远程主机上的网络流量。  
将它认为是窃听。窃听记录一个人在电话中说的每一句话，在电脑上发送的每一个数据包。这包括账户用户名、密码等等。  
这节我们将利用两种截然不同的方式监视网络流量。  
第一种我们将在局域网使用中间人攻击，使用arpspoof、urlsniff和driftnet。运用这些我们可以监视目标主机正在浏览的网页和图片。  
第二种我们通过Metasploit的session在远程主机上拦截数据包。我们将在wireshark和xplico中查看这些信息。  

## 使用arpspoof创建中间人攻击

中间人攻击的精华就是我们的Kali系统处于目标主机和路由器(网关)之间。这样，我们可以监视所有进出目标主机的流量。  
所有从目标主机流入互联网的流量都被路由到我们的主机，这就拦截了数据包并将它转发到局域网。所有从互联网进入目标主机的流量都先被路由到我们的主机，因此我们就可以拦截信息，之后将数据路由到目标主机。  
我们完成攻击要依靠重定义路由器和目标主机的ARP(Address Resolution Protocol)表。ARP表告诉系统MAC地址和IP地址的对应关系。因此我们重定义ARP表，告诉目标系统我是路由器(网关)并且告诉路由器我是目标主机。  

我们使用 arpspoof 可以很容易的重定义ARP表，但是首先我们需要打开IP转发。  
echo 1 > /proc/sys/net/ipv4/ip_forward  
现在我们运行 arpspoof ，我们需键入 网卡(-i)、目标主机(-t)、和路由器地址。  
arpspoof -i eth0 -t 192.168.198.132 192.168.198.2  

## 使用urlsnarf监视URL信息

让我们来看看目标正在浏览的URL地址。  
![alt text](/../static/img/blog/BasicSecurity_6/0.png)  
当目标浏览网页的时候，你就可以查看所有的URL流量。  

## 使用driftnet监视图片

![alt text](/../static/img/blog/BasicSecurity_6/1.png)  

## 使用Metasploit远程拦截数据包

如果我们通过一个exploit取得了一个Metasploit的Shell，我们可以记录下远程主机的网络流量。  

键入 run packetrecorder 查看选项。  
![alt text](/../static/img/blog/BasicSecurity_6/2.png)  
使用 选项 -li 查看远程主机上的所有网卡  
![alt text](/../static/img/blog/BasicSecurity_6/3.png)  
我们将攻击 2号网卡——Qualcomm WiFi adapter，使用 -l 来设置文件保存路径。  
![alt text](/../static/img/blog/BasicSecurity_6/4.png)  
使用 Ctrl + C 退出拦截。  

## Wireshark

现在我们有了数据包，我们要怎样使用它呢？Wireshark是一个强大的数据拦截和分析工具。  
To be continued...

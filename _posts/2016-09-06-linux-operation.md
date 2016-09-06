---
layout: post
title: "Linux运维之道"
desc: "Linux运维之道"
date: 2016-09-06
keywords: [Linux,Operation]
tags: [Linux,Operation]
categories: [Linux]
---

# 基本命令

## 文件操作

### pwd

描述：显示当前工作目录名称  
用法：pwd [option]  
选项：-P 显示链接的真实路径  

### cd

描述：切换当前工作目录  
用法：cd .. 切换工作目录到当前目录的上级目录  
           cd - 返回到 /usr/src 目录  
           cd 返回到 home 目录  

### ls

描述：显示目录与文件信息  
用法：ls[option]... [file/directory]  
用法：-a 显示所有信息(包括隐藏文件与目录)  
           -d 显示目录本身信息(而非目录下资料信息)  
           -h 人性化显示容量信息  
           -l 长格式显示详细信息  
           -c 显示文件/目录属性最后的修改时间  
           -u 显示文件/目录最后被访问的时间  
           -t 以修改时间排序(默认以文件名排序)  

### touch

描述：创建文件(如果文件不存在则创建，如果存在则更新文件时间为当前系统时间)  

### mkdir

描述：创建目录  
用法：mkdir [option]...[directory]  
选项：-p 创建多级目录  

### cp

描述：复制文件/目录  
用法：cp[option] 源 目标  
选项：-r 递归操作(复制子文件与子目录，一般复制目录)  

### rm

描述：删除文件/目录  
用法：rm [option]... 文件...
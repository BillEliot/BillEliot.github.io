---
layout: post
title: "Linux简单命令小结"
desc: "Linux简单命令小结"
date: 2016-08-31
keywords: "Linux,Console"
categories: [Linux]
tags: [Linux,Console]
---

## 基本命令
* **显示日期：date**
* **显示日历：cal**
* **数据同步写入磁盘：sync**
* **切换目录：cd dirname**
* **显示当前目录：pwd**
* **新建目录：mkdir**
* **删除目录：rmdir**

## 文件操作
* **寻找执行档：which [-a] commend**  
　　 -a：所有由PATH目录中找到的指令均列出  
* **修改文件所属群组：chgrp [-R] group filename**  
　　参数：-R 进行递归操作,通常变更一个目录下的所有文件  
* **修改文件拥有者：chown [-R] user filename**  
　　　　　　　　**chown [-R] user:group filename**  
　　参数：-R 进行递归操作,通常变更一个目录下的所有文件  
* **修改文件权限：chmod**  
　　参数：-R 进行递归操作,通常变更一个目录下的所有文件  
　　 **①：数字类型**  
　　　  chmod [-R] xyz filename  
　　 **①：符号类型**  
　　　  chmod [-R] (ugoa) (+-=) (rwx)  
* **压缩/解压缩**  

## 系统相关  
* **查看版本信息：uname -r**  
　　　　　　　**lsb_release -a**  

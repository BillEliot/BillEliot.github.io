---
layout: post
title: "Linux命令总结"
date: 2016-8-31
desc: "Linux命令总结"
keywords: "Linux,Console"
categories: [Linux]
tags: [Linux,Console]
icon: fa-database
---

## 基本命令
* **显示日期：date**
* **显示日历：cal**
* **数据同步写入磁盘：sync**

## 文件操作
* **修改文件所属群组：chgrp [-R] group filename**  
　　参数：-R 进行递归操作,通常变更一个目录下的所有文件
* **修改文件拥有者：chown [-R] user filename**
　　　　　　　　　**chown [-R] user:group filename**
　　参数：-R 进行递归操作,通常变更一个目录下的所有文件
* **修改文件权限：chmod**
　　* **①：数字类型**
　　　chmod [-R] xyz filename
　　* **①：符号类型**
　　　chmod [-R] (ugoa) (+-=) (rwx)

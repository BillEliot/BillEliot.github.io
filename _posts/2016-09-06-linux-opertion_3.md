---
layout: post
title: "Linux运维之道(4)"
desc: "Linux运维之道(4)"
date: 2016-09-09
keywords: [Linux,Opertion]
tags: [Linux,Opertion]
categories: [Linux]
---

# 自动化运维

## 命令历史

Bash会自动记录命令历史，用户执行的命令会在注销时自动记录到Home目录下的.bash_history文件中。通过history查看所有命令历史，命令历史都有记录编号。命令历史能够记录的信息数量由HISTSIZE所决定，默认 HISTSIZE=1000，命令历史最多纪录1000条，第1001条会把第1条覆盖。执行history -c 可以清空命令历史。  
使用 !string(string为关键字) 调用命令历史，如 !vim 将调用最后一次执行的以vim开头的命令；通过 !N(N为记录编号) 来准确定位历史记录；使用Ctrl+r打开搜索功能搜索相关记录。  

## 命令别名

别名可以把很长的命令简化缩写，大大提高工作效率。  
Example：alias 查看系统当前所有别名  
         

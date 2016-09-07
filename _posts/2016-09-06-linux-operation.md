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

*描述*：显示当前工作目录名称  
*用法*：pwd [option]  
*选项*：-P 显示链接的真实路径  

### cd

*描述*：切换当前工作目录  
*用法*：cd .. 切换工作目录到当前目录的上级目录  
              cd - 返回到 /usr/src 目录  
              cd 返回到 home 目录  

### ls

*描述*：显示目录与文件信息  
*用法*：ls[option]... [file/directory]  
*选项*：-a 显示所有信息(包括隐藏文件与目录)  
              -d 显示目录本身信息(而非目录下资料信息)  
              -h 人性化显示容量信息  
              -l 长格式显示详细信息  
              -c 显示文件/目录属性最后的修改时间  
              -u 显示文件/目录最后被访问的时间  
              -t 以修改时间排序(默认以文件名排序)  

### touch

*描述*：创建文件(如果文件不存在则创建，如果存在则更新文件时间为当前系统时间)  

### mkdir

*描述*：创建目录  
*用法*：mkdir [option]...[directory]  
*选项*：-p 创建多级目录  

### cp

*描述*：复制文件/目录  
*用法*：cp[option] 源 目标  
*选项*：-r 递归操作(复制子文件与子目录，一般复制目录)  

### rm

*描述*：删除文件/目录  
*用法*：rm [option]... 文件...
*选项*：-f 强制删除  
              -i 删除前提示是否删除  
              -r 递归删除(删除目录以及目录下的所有内容)  

### mv

*描述*：移动(重命名)文件或目录  

### find

*描述*：搜索文件或目录  
*用法*：find [命令*选项*] [路径] [表达式*选项*]  
*选项*：-empty 查找空白文件或目录  
              -group 按组查找  
              -name 按名称查找  
              -iname 按名称查找，不区分大小写  
              -mtime 按修改时间查找  
              -size 按大小查找  
              -type 按类型查找(文件 f 目录 d 设备 b,c 链接 l)  
              -user 按用户查找  
              -exec 对找到的档案执行命令  
              -a 且  
              -o 或  
Example：find -name hello.doc 查找当前目录下名为 hello.doc 的文件  
                  find /root -name "*.log" 查找 root 目录下所有后缀为 .log 的文件  
                  find -iname "Jacob" 不区分大小写查找当前目录下名为 Jacob 的文件
                  find / -empty 查找计算机中所有空文档  
                  find / -group tom 查找计算机中所有属组为tom的文件  
                  find / -mtime -3 查找计算机中所有 **3天内** 被修改过的文档  
                  find / -mtime +4 查找计算机中所有 **4天前** 被修改过的文件  
                  find / -mtime 2 查找计算机中 **2天前当天** 被修改过的文件  
                  find ./ -size +10M 查找当前目录下大于10MB的文件  
                  find ./ -type f 查找当前目录下所有普通文件  
                  find / -user tom 查找计算机中所有属于tom的文件  
                  find ./ -size +1M -exec ls -l {} \; 查找当前目录下大于1MB的文件后列出详细信息  
                  find /-size +1M -a -type f 查找计算机中所有大于1MB的普通文件  
                  
### du

*描述*：计算文件/目录容量  
*用法*：du [option]... [file/directory]  
*选项*：-h 人性化显示容量信息  
             -s 仅显示总容量  
           
## 查看文件内容

### cat

*描述*：查看文件内容  
*用法*：cat[option]... [file]...  
*选项*：-b 显示行号(空白行不显示行号)  
              -n 显示行号(包括空白行)  
              
### more

*描述*：分页查看文件内容(空格键查看下一页，q退出)  

### less

*描述*：分页查看文件内容(空格查看下一页，方向键上下回翻，q退出)  

### head

*描述*：查看文件头部内容(默认显示10行)  
*用法*：head [option]... [file]...  
*选项*：-c nK 显示文件前 nKB 的内容  
             -n 显示文件前 n行 内容  
             
### tail

*描述*：查看文件尾部内容(默认显示10行)  
*用法*：tail[option]... [file]...  
*选项*：-c nK 显示文件末尾 nKB 的内容  
             -n 显示文件末尾 n行 内容  
             -f 动态显示文件内容(Ctrl+C退出)  
             
### wc

*描述*：显示文件的行、单词、字节统计信息  
*用法*：wc[option]... [file]...  
*选项*：-c 显示文件字节统计信息
             -l 显示文件行数统计信息  
             -w 显示文件单次统计信息  
             
### grep

*描述*：查找关键词并打印匹配的行  
*用法*：grep[option] 匹配模式 [file]...  
*选项*：-i 忽略大小写  
             -v 取反匹配  
             -w 匹配单词  
             --color 显示颜色  
Example：grep th test.txt 在 test.txt 文件中过滤出包含 th 的行  
                  grep --color th test.txt 对匹配的关键字显示颜色  
                  grep -i the test.txt 过滤出包含 th 的行(不区分大小写)  
                  grep -w num test.txt 过滤单词 num  
                  grep -v the test.txt 过滤不包含 the 的行  
                  
### echo

*描述*：显示一行指定的文本  
*用法*：echo [option]... "string"...  
*选项*：-n 不输出换行(默认echo输出内容后换行)  
              -e 支持反斜线开始的转义字符  
                  \\ 反斜线  
                  \a  报警器  
                  \b  退格键  
                  \c  不生成格外输出  
                  \f  输入表单格式，换行后保留光标位置  
                  \n  换行  
                  \t  水平 Tab  
                  \v  垂直 Tab  
                  
## 链接文件

Linux 中的链接文件不同于Windows的快捷方式，Linux的链接文件分为**软链接**和**硬链接**，*软链接可以跨分区，但源文件不可删除；硬链接不可跨分区，但可以删除源文件.*  

* 软链接  

 ln -s /test/hello.txt /tmp/hi.txt 创建文件软链接
 ln -s /test/ /var/test 创建目录软链接  
 
 * 硬链接

ln /test/hello.txt /test/hi.txt  
rm /test/hello.txt 删除源文件后，硬链接仍可以正常使用  

## 压缩及解压缩


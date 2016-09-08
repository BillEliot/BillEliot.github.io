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

### gzip

*描述*：压缩与解压缩  
*用法*：gzip[option]... [file]...  
*选项*：-d 解压  
Example：gzip hello.txt 文件压缩后名为 hello.txt.gz  
                  gzip -d hello.txt.gz 解压gz文件  
                  
### bzip2

*描述*：压缩与解压缩  
Example：bzip2 hello.txt 文件压缩后名为 hello.txt.gz  
                  bzip2 -d hello.txt.gz 解压gz文件  

*gzip与bzip2 不可以对目录进行压缩操作*  

### tar

*描述*：压缩与解压缩  
*用法*：tar 模式 [option] [path]...  
*模式*：-c 创建压缩文件  
              --delete 从压缩文件中删除文件  
              -r 追加文件到打包文件  
              -t 列出打包文件的内容  
              -x 释放打包文件  
*选项*：-C 指定解压文件  
              -f 指定压缩后的文件名称  
              -j 打包后通过bzip2格式压缩  
              --remove-files 压缩后删除源文件  
              -z 打包后通过gzip格式压缩  
Example：tar -cf etc.tar /etc/ 将/etc/目录压缩保存为etc.tar  
                  tar -czf boot.tar.gz /boot/ 将/boot/目录压缩为 etc.tar.gz  
                  tar -cjf boot.tar.bz2 /tmp/ 将/tmp/目录压缩为 etc.tar.bz2  
                  tar --delete etc/hosts -f etc.tar 从压缩文件中删除文件 hosts  
                  tar -f etc.tar -r /root/install.log 追加文件到 etc.tar  
                  tar -tf boot.tar.gz 查看压缩文件文件信息  
                  tar -xzf boot.tar.gz 解压gz格式的压缩文件到当前目录  
                  tar -xjf etc.tar.bz2 解压bz2格式的压缩文件到当前目录  
                  tar -xzf boot.tar.gz -C /tmp 指定解压路径为 /tmp  
                  tar -czf mess.tar.gz /var/log/messages --remove-files 压缩后删除源文件  
                  
## 命令技巧

**Tab键补全命令；双按Tab键列出所有以当前字母开头的命令**  
**上、下键翻阅命令历史；history列出所有命令记录；!N N为命令编号，表示执行第N条历史命令**  
**Ctrl+L清屏**  
**which 命令 返回命令所在目录**  

## 帮助 查看命令手册

**man 命令**  
**info 命令**  
**命令 --help**  

## 账户与安全  

### 账户和组

Linux 对账号与组的管理是通过ID号实现的，登录系统时输入的用户名和密码，后台会将用户名转化为ID号后再判断该账号是否存在，并对比密码是否匹配。Linux 中用户ID称为UID，组ID成为GID。其中 UID为0代表超级管理员(root)，1~499之间的ID号会被系统预留，我们创建的普通用户ID号从500开始。  
Linux 的组有基本组和附加组。一个用户只可以加入一个基本组中，但可以同时加入多个附加组。创建用户时，系统默认自动创建同名的组并设置用户加入该基本组中。  

### 创建账号和组

使用 useradd 创建账户，groupadd 创建组账户。(创建账户和组需要root权限)  

### useradd

*描述*：创建账号  
*用法*：useradd [option] username  
*选项*：-c 设置账号信息(一般为账号全称)  
              -d 设置账号 家目录(默认为 /home/username)
              -e 设置账号失效日期，格式为 YYYY-MM-DD  
              -g 设置账号基本组  
              -G 设置账号附加组(多个附加组用逗号隔开)  
              -M 不创建账号的 家目录(一般与 -s 结合使用)  
              -s 设置账号登陆 Shell(默认为 bash)  
              -u 指定账号 UID
              
### groupadd

*描述*：创建组账号  
*用法*：groupadd [option] groupname  
*选项*：-g 设置 GID

### id

*描述*：显示账户和组信息

## 修改账户和组

### passwd

*描述*：更新账号认证信息  
*用法*：passwd [option] username  
*选项*：-l 锁定账号(仅root权限)  
              --stdin 从文件或管道读取密码  
              -u 解锁账号  
              -d 快速清空账号密码(仅root权限)  
              
### usermod

*描述*：修改账号信息  
*用法*：usermod [option] username  
*选项*：-d 修改账号 家目录  
              -e 修改账号失效日期  
              -g 修改账号所属基本组  
              -G 修改账号所属附加组  
              -s 修改账号登陆Shell  
              -u 修改账号 UID  
              
## 删除账户和组

### userdel

*描述*：删除账号  
*用法*：userdel [option] username  
*选项*：-r 删除账号的家目录  

### groupdel

*描述*：删除组账号  

## 账户与组文件解析

### 账户信息文件

账户信息保存在 /etc/passwd 文件中，可以通过 cat /etc/passwd 查看文件  
root:x:0:0:root:/root:/bin/bash  
文件以冒号为分隔符：  

* 第一列为账号名称  
* 第二列为密码占位符(x 表示该账号需要密码才可以登录，为空时账号无需密码即可登录)  
* 第三列为UID  
* 第四列为GID  
* 第五列为账号附加基本信息(一般存储账号全称、联系方式等信息)  
* 第六列为账号 家目录 位置  
* 第七列为账号登陆 Shell(/bin/bash 为可登陆系统Shell；/sbin/nologin表示账号无法登录系统)  

### 账号密码文件

账号密码信息保存在 /etc/shadow 文件中，可以通过 cat /etc/shadow 查看文件  
root:\$6\$05tfHji5$D5eGQtPWYdMU.d8RMjI./gGMRaa28cKfLhkhZKyUQrQB8FBjbMQ9rlCBK6Wu29sylwGklRrGPZsq9gdd0x0ti/:17049:0:99999:7:::
文件以冒号为分隔符：  

* 第一列为账号名称  
* 第二列为密码(未设置显示 !!；设置密码后加密显示)  
* 第三列为上次修改密码的时间距离 1970.01.01 几天  
* 第四列为密码最短有效天数(0表示无限制)  
* 第五列为密码最长有效天数(默认 99999 为无限期)  
* 第六列为过期前的警告天数(默认过期前7天警告)  
* 第七列为密码过期后的宽限天数(预留天数给账号修改密码，无法使用旧密码登录系统)  
* 第八列为账号失效日期(从 1970.01.01 起多少天后账号失效)  
* 第九列保留  

### 组账号信息文件

组账号信息保存在 /etc/group 文件中，可以通过 cat /etc/group 查看文件  
root:x:0:  
文件以冒号为分隔符：  

* 第一列为组账号名称  
* 第二列为密码占位符  
* 第三列为GID  
* 第四列为组成员(仅显示基本成员，附加成员不显示)  

### 组账号密码文件

组账号密码信息保存在 /etc/gshadow 文件中，可以通过 cat /etc/gshadow 查看文件  
root:*::  
文件以冒号为分隔符：

* 第一列为组账号名称  
* 第二列为组密码  
* 第三列为组管理员  
* 第四列为组成员(仅显示基本成员，附加成员不显示)  

**通过 gpasswd groupname 可以为组设置密码**  
**通过 gpasswd -A username groupname 可以为组添加管理员**  

## 文件/目录权限

Linux权限主要分为 读(r)、写(w)、执行(x) 三种控制，使用 ls -l 显示信息：  
total 516
drwxr-xr-x  2 root root   4096 Aug 11  2015 bin
drwxr-xr-x  5 root root   4096 Aug 11  2015 caf
drwxr-xr-x  2 root root   4096 Aug 11  2015 doc
drwxr-xr-x  5 root root   4096 Aug 11  2015 etc
-rw-r--r--  1 root root 279342 Aug 11  2015 FILES
-rw-r--r--  1 root root   2538 Aug 11  2015 INSTALL
drwxr-xr-x  2 root root   4096 Aug 11  2015 installer
drwxr-xr-x 15 root root   4096 Aug 11  2015 lib
drwxr-xr-x  3 root root   4096 Aug 11  2015 vgauth
-rwxr-xr-x  1 root root    243 Aug 11  2015 vmware-install.pl
-rwxr-xr-x  1 root root 205572 Aug 11  2015 vmware-install.real.pl

* 第一列第一字符为文件类型(-为普通文件；d为目录；l为链接文件；b/c为设备)  
* 第一列第二字符到第九字符为权限，三位一组分别为所有者权限、所属组权限、其他账号权限  
* 第二列为连接数量或子目录数量  
* 第三列为文档所有者  
* 第四列为文件的属组  
* 第五列为容量  
* 第六列为文件被修改的月份  
* 第七列为文件修改的日期  
* 第八列为文件被修改的时间  
* 第九列为文件/目录名称  

**权限的表示，除了可以用rwx表示外，还可以用数字表示**  

|数字|字符|文件|目录|
|:----:|:----:|:----:|:----:|
|4|R|查看文件内容|查看目录下的文件与子目录名称|
|2|W|修改文件内容|在目录下增、删、改、文件与子目录名称|
|1|X|可执行(程序或脚本)|可以cd进入该目录|

### 修改文件属性

#### chmod

*描述*：修改文件/目录权限  
*用法*：chmod [option] 权限 file/directory  
*选项*：--reference=RFILE 根据参考文档设置权限  
        -R 递归操作(修改所有子目录和子文件)  

**chmod参数中，u为所有者，g为属组，o为其他用户，a为所有人**  
Example：chmod u=rwx,g=rwx,o=rwx install.log  
         chmod a=rw install.log  
**使用字符修改权限的另一种形式是在原有权限的基础上修改权限，方法是使用 +/- 权限的方法**  
Example：chmod g-x,o-wx install.log  
         chmod --reference=install.log.syslog install.log 以 install.log.syslog 为参考修改 install.log 的权限  
         
#### chown

*描述*：修改文件/目录的所有者和属组
*用法*：chown [option][所有者][:[属组]] file/directory  
*选项*：-R 递归操作(修改所有子目录和子文件)  
Example：chown user2:mail install 修改文件的所有者为user2，属组为mail  
         chown :root install 仅修改文件属组为 root  
         chown root install 仅修改文件所有者为 root

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
         alias h5='head -5' 定义新的别名  
         unalias h5 取消别名定义  

## 管道与重定向

标准输入文件描述符为0，标准输出文件描述符为1，错误输出的文件描述符为2。Linux中可以使用重定向符 <,>,<<,>>,| 重新定义输入与输出。  
管道将一个命令的标准输出重定向给下一个命令，并作为该命令的标准输入。  
输出重定向可以使用 >或>> ，使用 > 可以将输出导出到文件，如果文件不存在则创建，如果存在则覆盖该文件内容；使用 >> 可以将输出追加至文件；对应错误信息的重定向需要使用 2> 或 2>> 实现。  
输入重定向可以使用 < 可以从文件中提取输入信息。  
Example：echo "pass" | passwd --stdin tom 设置tom的密码为pass  
         ls > list.txt 将输出保存至list.txt  
         hostname >> list.txt 将输出追加至list.txt文件末尾  
         mail -s test XX@gmail.com < list.txt 发送邮件，内容来自文件  
         ls -l abc install.log 2> error.txt 仅将错误信息重定向  
         ls -l abc install.log &> error.txt 标准输出与错误输出均导入至error.txt  
         
## Bash 快捷键

|快捷键|描述|
|------|----|
|Ctrl+a|光标移动到行首|
|Ctrl+e|光标移动到行尾|
|Ctrl+f|光标右移一个字符|
|Ctrl+b|光标左移一个字符|
|Ctrl+l|清屏|
|Ctrl+u|删除光标至行首的字符|
|Ctrl+k|删除光标至行尾的字符|
|Ctrl+c|终止进程|
|Ctrl+z|挂起进程(jobs命令查看挂起的进程)|
|Ctrl+w|删除光标前一个单词(空格为分隔符)|
|Ctrl+d|删除光标后一个单词|

Linux中提供了一个特殊设备：/dev/null，任何往里写入的信息都将消失。对于大量无意义的输出信息，可以通过管道导入/dev/null设备。  
Example：echo "pass" | passwd --stdin tom > /dev/null  

id tom >> user 2>> error  分离重定向，如果账号tom存在，则相关信息记录在user文件中，否则记录在error文件中。  

## 命令序列

Linux 中，可以使用控制字符 ;,&&,||,& 控制命令的执行方式，& 使命令开启一个子Shell，并在后台执行；使用 ; 可以把多个命令组合，多个命令没有逻辑上的关系，仅按照顺序执行；使用 && 将多个命令组合，仅当前命令执行成功后才会执行 && 后面的命令；|| 的作用于与 && 的作用相反，仅当前命令执行失败后才会执行 || 后面的命令。  
Example：firefox 火狐浏览器前端启动，导致当前Shell暂时无法使用  
         firefox & 火狐浏览器后台启动，不影响当前Shell使用  
         ls /tmp ; ls /root ; ls /home 所有命令按顺序执行  
         ls text.txt && cat text.txt 如果文件存在则显示文件内容，否则报错  
         gedit || vim 如果有gedit编辑器则打开，否则打开vim编辑器  
         id root &> /dev/null && echo "Hello administrator!" || echo "No such user"  
## 作业控制

可以通过 & 将进程放入后台执行，或者命令执行后通过Ctrl+Z将进程放入后台并暂停执行，可以使用jobs命令查看后台进程，系统会为每一个这样的进程分配编号，通过 fg index 命令将后台进程再次调回前台执行。  
Example：firefox &  
         jobs  
         Running                 firefox &  
         fg 1  
         
## 花括号{}使用

Example：echo {a,b,c} 输出 a b c  
         echo user{1,5,8} 输出 user1 user5 user8  
         echo {0..10} 输出 0 1 2 3 4 5 6 7 8 9 10  
         echo {0..10..2} 输出 0 2 4 6 8 10  
         echo a{2..-1} 输出 a2 a1 a0 a-1  
         mkdir /tmp/{dir1,dir2,dir3}  
         ls -ld /tmp/dir{1,2,3}  
         chmod 777 /tmp/dir{1..3}  

# 变量

变量具有一个值，以及零个或多个属性。定义变量的语法格式：  
name=[value]  
如果value没有指定，变量将赋值为空字串，使用 $name 调用变量值。通过 typeset 为变量添加属性。  
Example：NAME=Eliot  
         echo $NAME  
Eliot  
         typeset -r NAME 添加 readonly 属性  
         NAME=IEliot  
bash: NAME: readonly variable  

如果预先定义一个变量但暂时不赋值给它，使用 declare 命令。  
Example：declare NUMBER  
         typeset -i NUMBER 设置为整数变量  

通过 read 命令设置变量，read 从标准输入中读取变量值，使用 -p 添加相应的提示信息。  
Example：read NUMBER  
20  
         echo $NUMBER   
20  
         read -p "Please input a number:" NUMBER  
Please input a number:30  
         echo $NUMBER  
30  
         set 查看系统中设置的所有变量及值  
         unset NUMBER 删除变量  
         
## 变量的使用范围

使用 name=[value] 形式定义的变量默认仅在当前Shell中有效，子进程不会继承变量。  
使用 export 将变量放入环境中，新进程会从父进程继承环境，export可以直接定义环境变量并赋值。  
Example：TEST=pass  
         echo $TEST  
         bash 当前Shell下开启新进程bash  
         echo $TEST 查看变量值为空  
         exit  
         export TEST 添加至环境变量  
         export NAME=tom 直接定义环境变量  
         
## 环境变量

bash预设的环境变量：  

|变量名|含义|
|:----:|:--:|
|BASHPID|当前bash的PID|
|GROUPS|当前用户属组的GID|
|HOSTNAME|当前主机名|
|PWD|当前工作目录|
|OLDPWD|前一个工作目录|
|RANDOM|0~32767之间的随机数|
|UID|当前用户ID|
|HISTSIZE|命令历史记录的最多条数|
|HOME|当前用户家目录|
|PATH|命令搜索路径|
|PS1|主命令提示符|
|PS2|次命令提示符|

Linux 系统通过PATH变量搜索命令，PATH变量的值就是目录集合，系统会按照顺序查找这些命令，如果找不到则提示未找到。  
Example：echo $PATH
         /usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/root/bin
目录以 : 分隔  
对PATH修改时，不可直接赋值，这会覆盖掉PATH的值，而要在原值后追加新值。  
Example：PATH=$PATH:/root (因为并没有写入文件，这只是临时修改，重启终端后就会复原)  
## 位置变量

位置变量在脚本中可以调用运行脚本时不同位置的参数，参数以空格分隔，$0 代表当前脚本程序文件名，$1 代表脚本程序的第一个参数，$2 为第二个参数，以此类推($1~$9)。$# 代表脚本程序所有参数个数；$* 与 $@ 代表所有参数的内容，区别是 $*将所有参数作为一个整体，$@ 将所有参数分别作为个体看待；$$ 代表当前进程的 ID；$? 表示程序的退出码(0 表示执行成功；非0 表示失败)。  
Example：编写一个脚本  
vi /root/Shell.sh 写Shell  
\#!/bin/bash  
echo "The file name : $0"  
echo "The first parameter : $1"  
echo "The second parameter : $2"  
echo "The number of all parameters are : $#"  
echo "All parameters: $*"  
echo "All parameters: $@"  
echo "PID : $$"  
bash Shell.sh a b c 执行Shell  
The file name : Shell.sh  
The first parameter : a  
The second parameter : b  
The number of all parameters are : 3  
All parameters: a b c  
All parameters: a b c  
PID : 6128  

## 变量的展开替换

Linux 中可以使用 ${变量名} 的形式展开变量的值，一个变量为 NAME=Eliot ，可以使用 echo ${NAME} 显示该变量值，除此之外还有更丰富的变量展开功能。  
${varname:-word} 如果 varname 存在并且非 null，则返回其值，否则返回word  
${varname:=word} 如果 varname 存在并且非 null，则返回其值，否则设置为word  
${varname:?word} 如果 varname 存在并且非 null，则返回其值，否则返回varname:word  
${varname:+word} 如果 varname 存在并且非 null，则返回word，否则返回null  
Example:NAME=Eliot  
        echo ${NAME:-no user}  
        echo ${NAME_B:-no user}  
        echo ${NAME:=no user}  
        echo ${NAME_B:=no user}  
        echo ${NAME:?no defined}  
        echo ${NAME_B:?no defined}  
        echo ${NAME:+OK}  
        echo ${NAME_B:+ERROR}  

${varname#key} 从头开始删除关键字，执行最短匹配  
${varname##key} 从头开始删除关键字，执行最长匹配  
${varname%key} 从尾开始删除关键字，执行最短匹配  
${varname%%key} 从尾开始删除关键字，执行最长匹配  
${varname/old/new} 将 old 替换为 new ，仅替换第一个出现的 old  
${varname//old//new} 将 old 替换为 new ，替换所有 old  
Example：

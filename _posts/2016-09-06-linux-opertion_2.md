---
layout: post
title: "Linux运维之道(3)"
desc: "Linux运维之道(3)"
date: 2016-09-09
keywords: [Linux,Opertion]
tags: [Linux,Opertion]
categories: [Linux]
---

# 软件管理

## Linux常用软件包类型

Linux 中的软件包类型丰富多样，很多特定的软件包格式仅适用于特定的Linux发行版。目前流行的软件包格式有：可直接执行的RPM与DEB、源代码形式的gzip与bzip2压缩包。  

## 源码编译安装软件

步骤：  

* 软件源码一般都会被打包压缩，常见格式有 .tar.gz 和 .tar.bz2 格式。我们需要将压缩包解压。  
* 运行 configure 脚本。通过特定的选项修改软件设置与功能，该脚本一般可以用来指定安装路径、开启/关闭软件的特定功能。脚本选项可以通过阅读安装说明或./configure --help查看软件支持的功能选项(有些软件没有提供configure脚本，可以直接执行make进行编译)。  
* 运行 make 命令将源代码编译为计算机可识别的机器语言，执行configure脚本时就会检查对应的工具是否已安装(常用的开发工具为：gcc、python、perl、make、automake)。  
* 通过 make install 将软件安装到指定位置。  

## 服务管理

服务一般是在后台运行的进程，有些是系统服务，有些是独立的网络服务。我们可以通过运行主程序来启动或关闭服务，也可以通过系统提供的 service 以及chkconfig 命令管理大量服务。  

### 通过主程序管理服务

通过手动执行主程序来启动服务，例如手动启动Apache仅需知道Apache主程序的存储路径即可，可以执行 /usr/local/apache2/bin/apachectl start 开启服务，并通过 /usr/local/apache2/bin/apachectl stop 关闭服务。  

### 通过 service 管理服务

我们可以通过系统提供的 service 来管理服务。通过 service 来管理 ssh远程连接服务。  
service sshd start 开启服务  
service sshd stop  关闭服务  
service sshd status 查看服务当前状态  

修改服务配置后，在不想关闭服务的情况下使新的配置生效，可以通过 reload 来重新加载配置文件；修改服务配置后，不知道新的设置是否正确，可以使用 condrestart，此时系统会测试新的配置文件是否有问题，没问题才会重启服务。  
service sshd reload  
service sshd condrestart  

### 开机启动服务

Linux系统可以在六种模式下启动：  

* 0 关机模式  
* 1 用户模式  
* 2 无NFS网络字符界面模式  
* 3 全功能字符界面模式  
* 4 未定义  
* 5 图型模式  
* 6 重启模式  

**通过runlevel查看当前所处的运行级别；通过 init N(N为运行级别) 修改运行级别**  

chkconfig 命令用来管理开机启动项，设置开机启动的时候需要指定运行级别  
*描述*：更新/查询系统服务的运行级别信息  
*用法*：chkconfig 指令 [服务名称] [on/off]  
*指令*：--list 查询系统服务运行级别信息  
        --level 指定操作的运行级别(默认2345级别)
**chkconfig仅适用于RedHat系列，Debian使用 apt-get install sysv-rc-conf安装使用sysv-rc-conf**  
Example：chkconfig --list sshd 查看sshd服务所有的运行级别信息  
         chkconfig --list 查看所有服务所有的运行级别信息  
         chkconfig --level 15 sshd off 设置sshd服务在1、5级别开机不启动  
         chkconfig sshd on 设置sshd服务在2、3、4、5级别开机启动  
         
# 计划任务

## at一次性计划任务

使用at指定一次性计划任务前确保atd服务开启，否则计划任务不会被执行。  
*service atd start 开启服务；chkconfig atd on确保该服务开机启动*  

at命令  
*描述*：在指定的时间执行特定命令  
*用法*：at time  
*选项*：-m 计划任务结束后发送邮件给用户  
        -l 查看用户计划任务  
        -d 删除用户计划任务  
        -c 查看at计划任务具体内容  
Example：at 23:11 指定将在当天23:11执行计划任务  
         at> tar -cjf log.tar.bz2 /var/log 计划任务内容  
         at> shutsown -h now 计划任务内容  
         at> <EOT> 可以输入多条命令，输入完毕后Ctrl+D快捷键结束  
         at -l 查看计划任务  
         at -c 1 查看编号为1的计划任务的具体内容  
         at -d 1删除编号为1的计划任务  
at 可以使用的时间格式有很多，at 小时:分钟(默认代表当天时间)，at 4pm+3days(代表3天后的下午4点执行计划任务)，at 12:00 2014-12-12(指定年月日及日期的计划任务)...  

## cron 周期性计划任务

使用cron指定周期性计划任务前确保cron服务开启，否则计划任务不会被执行。  
*service cron start 开启服务；chkconfig cron on确保该服务开机启动*  

crontab命令  
*描述*：维护周期性的计划任务  
*用法*：crontab [-u 用户] [-l|-r|-e]  
*选项*：-u 指定计划任务的用户(默认为当前用户)  
        -l 查看计划任务  
        -r 删除计划任务  
        -e 编辑计划任务  
        -i 使用-r删除计划任务时，要求用户确认删除  
cron计划任务格式如表所示。  
|第一列|第二列|第三列|第四列|第五列|第六列|
|------|------|------|------|------|------|
|分|时|日|月|周|命令|
使用24小时制，分钟范围为00~59，小时范围为00~23，日期范围为1~31，月范围为1~12，周范围为0~7(0、7都可表示周日)，如需指定时间段用 - 表示连续的时间，使用 , 表示若个不连续的时间，使用 * 表示所有时间，使用 / 表示间隔时间。  
Example：crontab -e 为当前用户设置计划任务  
         23 23 * * 5 tar -czf log.tar.bz2 /var/log #每周5晚23:23执行日志备份  
         00 */3 * * * who #每3小时的整点检查用户登陆情况  
         00 10 * * 3,5 free | mail -s "Mem" admin@XX.com #每周3、周5的10点将系统内存信息发送管理员邮箱  

## 计划任务权限

为了控制用户随意定义自己的计划任务，管理员可以进行ACL访问控制。at计划任务的控制文件分别为 /etc/at.allow(默认不存在) 和 /etc/at.deny，cron计划任务的控制文件分别为 /etc/cron.allow(默认不存在) 和 /etc/cron.deny。  
当allow文件存在时，仅在allow文件中出现的用户可以使用对应的计划任务，如果allow文件和deny文件同时存在内容一样的账号，则仅出现在allow中的用户可以使用计划任务。如果没有allow文件而仅有deny文件，则所有出现在deny中的用户无法使用计划任务，其他所有用户均可以使用计划任务。  

# 性能监控

## 监控CPU —— uptime

uptime  
03:33:30 up  6:51,  3 users,  load average: 0.11, 0.11, 0.07  
*描述*：打印当前时间；系统运行时间；当前登录用户数；系统平均负载  
**CPU负载分别显示最近一分钟、五分钟、十五分钟的负载情况。负载表示单位时间段内CPU等待队列中平均有多少进程在等待，等待的进程个数越多就说明CPU越忙**  

## 监控内存及交换分区 —— free

*描述*：显示系统内存及交换分区信息  
*用法*：free [option] (默认 -k)  
*选项*：-h 人性化显示输出容量  
        -b 输出容量为B  
        -k 输出容量为KB  
        -m 输出容量为MB  
Example：free -h
         total       used       free     shared    buffers     cached  
Mem:     494M        468M       26M      5.1M      21M         199M  
-/+ buffers/cache:   247M       247M  
Swap:    1.0G        7.2M       1.0G  
**Linux开机后会预先提取一部分内存并划分为buffer和cache以后随时提供给进程使用**  
第一行 total表示内存总量为494MB，used表示系统将内存中468MB划分成了buffer和cache(也就是buffer和cache的总容量)；free代表内存总量减去buffer和cache的总和之后的剩余容量为26MB；buffers表示当前buffer的剩余容量为21MB；cached表示当前cache的剩余容量为199MB；  
第二行 used表示buffer和cache当前共使用247MB；free表示buffer和cache总剩余容量加未被分配内存之和(247MB = 21MB + 199MB + 26MB)，该值为系统中内存剩余的实际容量。  
第三行为交换分区的使用情况，total表示交换分区总容量为1GB；used 表示已经使用了7.2MB；free表示剩余交换区为1GB  

## 监控磁盘使用情况 —— df

*描述*：输出系统磁盘空间的使用量信息  
*用法*：df [option]  
*选项*：-h 人性化显示容量信息  
        -i 显示磁盘inode使用量信息  
        -T 显示文件系统类型  
Example：df -hT  
Filesystem     Type      Size  Used Avail Use% Mounted on  
/dev/sda1      ext4       39G  3.5G   33G  10% /  
udev           devtmpfs   10M     0   10M   0% /dev  
tmpfs          tmpfs      99M  5.0M   94M   6% /run  
tmpfs          tmpfs     248M   68K  248M   1% /dev/shm  
tmpfs          tmpfs     5.0M  4.0K  5.0M   1% /run/lock  
tmpfs          tmpfs     248M     0  248M   0% /sys/fs/cgroup  
tmpfs          tmpfs      50M  4.0K   50M   1% /run/user/116  
tmpfs          tmpfs      50M  8.0K   50M   1% /run/user/0  
根分区类型为 ext4 ，总容量为39GB，已经使用3.5GB，剩余33GB可用，使用率为10%，挂载点为/  
df -i  
Filesystem      Inodes  IUsed   IFree IUse% Mounted on  
/dev/sda1      2555904 127179 2428725    5% /  
udev             61162    354   60808    1% /dev  
tmpfs            63285    527   62758    1% /run  
tmpfs            63285      2   63283    1% /dev/shm  
tmpfs            63285      6   63279    1% /run/lock  
tmpfs            63285     13   63272    1% /sys/fs/cgroup  
tmpfs            63285      6   63279    1% /run/user/116  
tmpfs            63285     10   63275    1% /run/user/0  
根分区inode总个数为2555904，已经使用127179个，剩余2428725个，使用率为5%。**icode决定了该分区可以创建文件的个数，有多少个icode，就可以在该分区创建多少个文件。当icode用尽时，即使分区容量 >0GB，也无法创建新文件。**  

## 监控网络使用情况 —— ifconfig和netstat

### ifconfig
查看网卡接口信息，Linux中以太网卡一般被标识为ethx，第一块网卡为eth0，第二块网卡为eth1，以此类推。一般使用 netstat 查看服务器开启的端口信息以及网络连接状态。  
eth0      Link encap:Ethernet  HWaddr 00:0c:29:0c:fc:a9  
          inet addr:192.168.44.128  Bcast:192.168.44.255  Mask:255.255.255.0  
          inet6 addr: fe80::20c:29ff:fe0c:fca9/64 Scope:Link  
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1  
          RX packets:7779 errors:0 dropped:0 overruns:0 frame:0  
          TX packets:532 errors:0 dropped:0 overruns:0 carrier:0  
          collisions:0 txqueuelen:1000   
          RX bytes:886393 (865.6 KiB)  TX bytes:57840 (56.4 KiB)  
          Interrupt:19 Base address:0x2000  
该网卡地址为192.168.44.128 广播地址为192.168.44.255 子网掩码为 255.255.255.0；系统开机到现在总共接受了7779个数据包，总共发送532个数据包；计算机开机到现在总共接受865.6KB数据，总共发送56.4KB数据；  

### netstat
*描述*：输出网络连接、路由表、网络接口统计等信息  
*用法*：netstat [option]  
*选项*：-s 显示各种协议统计信息  
        -n 使用数字形式的IP、端口、用户ID代替主机、协议、用户名称  
        -p 显示进程名称及对应PID  
        -l 仅显示正在监听的socket接口信息  
        -u 查看udp连接信息  
        -t 查看tcp连接信息  

## 监控进程 —— ps和top

### ps
*描述*：查看当前进程信息  
*用法(BSD)*：ps -ax 查看所有进程信息  
             ps -axu 全格式显示进程信息  
Example：
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND  
root         1  0.0  0.9  28832  4676 ?        Ss   Sep08   0:01 /sbin/init  
root         2  0.0  0.0      0     0 ?        S    Sep08   0:00 [kthreadd]  
root         3  0.0  0.0      0     0 ?        S    Sep08   0:01 [ksoftirqd/0]  
root         5  0.0  0.0      0     0 ?        S<   Sep08   0:00 [kworker/0:0H]  
root         7  0.0  0.0      0     0 ?        S    Sep08   0:02 [rcu_sched]  
root         8  0.0  0.0      0     0 ?        S    Sep08   0:00 [rcu_bh]  
root         9  0.0  0.0      0     0 ?        S    Sep08   0:00 [migration/0]  
root        10  0.0  0.0      0     0 ?        S    Sep08   0:01 [watchdog/0]  
root        11  0.0  0.0      0     0 ?        S<   Sep08   0:00 [khelper]  
root        12  0.0  0.0      0     0 ?        S    Sep08   0:00 [kdevtmpfs]  
root        13  0.0  0.0      0     0 ?        S<   Sep08   0:00 [netns]  
root        14  0.0  0.0      0     0 ?        S    Sep08   0:00 [khungtaskd]  
root        15  0.0  0.0      0     0 ?        S<   Sep08   0:00 [writeback]  
root        16  0.0  0.0      0     0 ?        SN   Sep08   0:00 [ksmd]  
...

* USER代表进程的执行用户  
* PID为进程唯一标识  
* PPID表示父进程ID  
* %CPU表示进程的CPU占用率  
* %MEM表示进程的内存占用率  
* VSZ表示进程所使用的虚拟内存大小(单位KB)  
* RSS表示进程使用的真是内存大小(单位KB)  
* TTY为终端  
* START表示进程启动时间  
* STAT表示进程状态(D：不可中断；R正在运行；S：正在睡眠；T停止或被追踪；X：死进程；Z僵尸进程)  
* TIME表示进程占有CPU总时间  
* COMMAND表示进程命令  

### top

*描述*：动态查看进程信息  
*选项*：-d 更新时间(默认3秒)  
        -p 查看指定PID的进程信息  
Example：
top - 04:45:35 up  8:03,  3 users,  load average: 0.01, 0.04, 0.05  
Tasks: 132 total,   1 running, 131 sleeping,   0 stopped,   0 zombie  
%Cpu(s):  3.4 us,  0.3 sy,  0.0 ni, 96.2 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st  
KiB Mem:    506284 total,   494428 used,    11856 free,    24464 buffers  
KiB Swap:  1045500 total,     7340 used,  1038160 free.   207224 cached Mem  

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND  
  959 root      20   0  225396  54496  21220 S  4.3 10.8   0:32.37 Xorg  
 4223 root      20   0  403876  23756  19244 S  1.3  4.7   0:03.27 xfce4-terminal  
 1573 root      20   0  225616  27424  23680 S  0.3  5.4   0:31.37 vmtoolsd  
    1 root      20   0   28832   4676   2928 S  0.0  0.9   0:01.75 systemd  
    2 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kthreadd  
    3 root      20   0       0      0      0 S  0.0  0.0   0:01.37 ksoftirqd/0  
    5 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kworker/0:0H  
    7 root      20   0       0      0      0 S  0.0  0.0   0:03.16 rcu_sched  
    8 root      20   0       0      0      0 S  0.0  0.0   0:00.00 rcu_bh  
   14 root      20   0       0      0      0 S  0.0  0.0   0:00.01 khungtaskd  
   15 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 writeback  
   16 root      25   5       0      0      0 S  0.0  0.0   0:00.00 ksmd  
   17 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 crypto  
top 默认按CPU使用率排序，键入M按照内存使用率排序，键入N按照进程号排序，键入z高亮显示颜色  

# 网络配置

Linux 都可以通过两种方式为系统设置网络参数：通过命令设置，这种方式优点是方便、快捷，并且设置后立刻生效，但计算机重启后所有设置均失效；通过修改系统配置文件实现，这种方式优点是设置后永久保存，重启后仍有效，但这样的设置不会立刻生效，需重启计算机或相关服务才可使其生效。  

## 命令行设置网络参数 —— ifconfig

*描述*：显示/设置网络接口信息  
*用法*：ifconfig interface 选项 地址  
Example：ifconfig eth0 192.168.0.31 netmask 255.255.255.0 设置eth0网卡接口IP为192.168.0.31，子网掩码为255.255.255.0  
         ifconfig eth0 查看eth0网卡接口信息  
         ifconfig eth0 down 关闭eth0网卡接口  
         ifconfig eth0 up 开启eth0网卡接口  
         
## 主机名 —— hostname

*描述*：显示/设置系统主机名称  
*用法*：hostname [option]  
Example：hostname 查看主机名称  
         hostname debian 设置主机名称  
         hostname -i 查看本机IP信息(读取 /etc/hosts 获取本机IP地址)  
         
## 路由参数 —— route

*描述*：显示/设置静态IP路由表  
*用法*：route [option] 查看路由信息  
        route add 目标网络 gw 网关地址 添加路由表信息  
        route del 目标网络  删除路由表信息  
        route 查看当前路由表  
        route -n 使用数字地址代替主机名称  
        route add default gw 192.168.0.254 添加默认网关为 192.168.0.254  
        route add -net 172.16.0.0/16 gw 192.168.0.254 添加指定网段的网关  
        route add -net 192.56.76.0 netmask 255.255.255.0 dev eth0 添加路由记录，指定通过eth0网卡传输到192.56.76.0网段数据  
        route del default gw 192.168.0.254 删除默认网关  
        route del -net 172.16.0.0/16 删除指定网段的网关记录  
        
## 网络故障排错

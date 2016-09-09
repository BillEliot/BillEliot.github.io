---
layout: post
title: "Linux运维之道(2)"
desc: "Linux运维之道(2)"
date: 2016-09-08
keywords: [Linux,Opertion]
tags: [Linux,Opertion]
categories: [Linux]
---

## ACL访问控制权限

由于系统的基本权限是针对文件所有者、属组或其他账号进行控制，无法针对某个单独的账号进行控制，所以就有了ACL(Account Control List)访问控制列表的概念。使用ACL就可以针对单一账号设置文件的访问权限。  
Linux系统使用 getfacl 查看文件的ACL权限，使用 setfacl设置文件的ACL权限。  

getfacl 输出内容：  
getfacl INSTALL  
\# file: INSTALL  
\# owner: root  
\# group: root  
user::rw-  
user:eliot:rwx  
group::r--  
group:cool:r-x  
mask::r-x  
other::r--  

* 第一行为文件或目录名称  
* 第二行为文件所有者  
* 第三行为文件属组  
* 第四行为 suid、sgid、sticky 权限标记位  
* 第五行为文件所有者权限  
* 第七行为文件属组权限  
* 第十行为其他账号权限  
* 第五、七、十行为系统基本权限，第六行为通过ACL添加的对账号的访问控制权限，第八行为通过ACL添加的对组账号的访问权限，第九行为权限掩码  

---

getfacl INSTALL  
\# file: INSTALL  
\# owner: root  
\# group: root  
user::rw-  
group::r--  
other::r--  

从以上信息可以看出，该文件未设置附加的ACL访问控制列表，仅有基本的文件所有者、属组、其他账号的访问控制权限。  

### setfacl

*描述*：设置文件访问控制列表  
*用法*：setfacl [option] [ACL信息] file/directory  
*选项*：-b 删除所有附加的ACL信息  
        -k 删除默认的ACL信息  
        -m 添加ACL信息  
        -x 删除指定的ACL信息  
        -R 递归处理所有子文件与子目录  
Example：setfacl -m u:user1:rw test.txt  添加ACL信息：使用户user1对test.txt文件可读可写  
         setfacl -m g:user1:r test.txt  添加ACL信息：使组user1对test.txt文件可读  
         setfacl -x u:user1 text.txt  删除账号user1的ACL信息  
         setfacl -x g:user1 text.txt  删除组user1的ACL信息  
         setfacl -b test.txt  删除所有附加的ACL信息  
         
# 存储管理

Linux 根据设备类型对存储设备进行识别，IDE存储设备被识别为hd，第一个IDE设备被识别为hda，第二个IDE设备被识别为hdb，以此类推；SATA,USB,SCSI设备会被识别为sd，同样第一个设备为sda，第二个为sdb，以此类推。  
对于分区，Linux使用数字表示。第一块SATA硬盘第一个分区为sda1，第二块SATA硬盘的第二个分区为sdb2。  

## 磁盘分区

传统的MBR分区方式是一块硬盘最多可以分四个主分区，即使硬盘还有剩余空间也无法继续分区。  
![alt text](/../static/img/blog/0.png)  
传统的MBR分区方式中，如果需要更多的分区就需要使用 扩展分区中创建逻辑分区 的方法实现。如图，此时可以在扩展分区中划分多个逻辑分区，所有逻辑分区的总和为扩展分区的大小。逻辑分区一定是以编号5开始的(SATA硬盘的第一个逻辑分区一定是 sda5)；  
![alt text](/../static/img/blog/1.png)  

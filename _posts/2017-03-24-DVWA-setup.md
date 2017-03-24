---
layout: post
title: "【DVWA】简介以及安装"
date: 2017-3-24
keywords: "web, penetration"
categories: [web]
---


**搭建环境：Kali Linux**

## 系统介绍

`DVWA（Damn Vulnerable Web Application）`是randomstorm的一个开源项目，用来进行安全脆弱性鉴定的PHP/MySQL Web应用，旨在为安全专业人员测试自己的专业技能和工具提供合法的环境，帮助web开发者更好的理解web应用安全防范的过程。  

DVWA共有十个模块:  
1. Brute Force（暴力破解）
2. Command Injection（命令行注入）
3. CSRF（跨站请求伪造）
4. File Inclusion（文件包含）
5. File Upload（文件上传）
6. Insecure CAPTCHA （不安全的验证码）
7. SQL Injection（SQL注入）
8. SQL Injection（Blind）（SQL盲注）
9. XSS（Reflected）（反射型跨站脚本）
10. XSS（Stored）（存储型跨站脚本）

> DVWA 1.9的代码分为四种安全级别：Low，Medium，High，Impossible。学习者可以通过比较四种级别的代码，接触到一些PHP代码审计的内容。  

## 安装

#### 安装xampp

[点击下载xampp](http://www.apachefriends.org)  

> `XAMPP` 是一个易于安装且包含 MySQL、PHP 和 Perl 的 Apache 发行版。XAMPP 的确非常容易安装和使用：只需下载，安装，启动即可。

```python
cd Downloads
chmod o+x chmod o+x xampp-linux-x64-5.6.30-0-installer.run
./xampp-linux-x64-5.6.30-0-installer.run
rm -f xampp-linux-x64-5.6.30-0-installer.run
```
启动所有服务  
*Starting MySQL Database...
Starting ProFTPD...
Checking syntax of configuration file
/opt/lampp/proftpd/scripts/ctl.sh : proftpd started
/opt/lampp/mysql/scripts/ctl.sh : mysql  started at port 3306*

> 使用http://localhost 或 http://127.0.0.1 测试服务是否启动

#### 部署DVWA

[点击下载DVWA](www.dvwa.co.uk)  

```python
cd Downloads
unzip DVWA-master.zip
rm -rf /opt/lampp/htdocs/*
cp -r DVWA-master/. /opt/lampp/htdocs
```

编辑数据库信息  

```python
cd /opt/lampp/htdocs
vim config/config.inc.php
```

*$_DVWA = array();
$_DVWA[ 'db_server' ]   = '127.0.0.1';
$_DVWA[ 'db_database' ] = 'dvwa';
$_DVWA[ 'db_user' ]     = 'root';
$_DVWA[ 'db_password' ] = ''*

http://localhost 开始创建数据库

> 默认账户密码 admin password

**至此，DVWA搭建完毕**

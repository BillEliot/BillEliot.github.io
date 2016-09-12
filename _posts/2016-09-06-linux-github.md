---
layout: post
title: "Linux下github使用"
desc: "Linux下github使用"
date: 2016-09-12
keywords: [Linux,github]
tags: [Linux,github]
categories: [Linux]
---

* 生成 ssh key  

ssh-keygen -t rsa -C "email@email.com"(默认保存在/root/.ssh/id_rsa.pub)  

* 添加SSH key

到github，进入 settings -> SSH and GPG Keys  
点击New SSH key，写入Title，粘贴刚刚生成的 SSH keys，点击 Add SSH Key  

* 测试SSH是否连接成功  

ssh -T git@github.com  
如果出现You’ve successfully authenticated, but GitHub does not provide shell access  表示已成功连上github。如果出现Agent admitted failure to sign using the key.Permission denied (publickey)错误 执行 ssh -add  

* 配置 Git 文件  

git config --global user.name "your name"      //配置用户名  
git config --global user.email "your email"    //配置email  

* 从 git clone仓库

git clone 仓库的URL  

* 从 git 从本地上传文件到 github  

进入本地仓库 git init(执行一次即可)  
git remote add origin git@github.com:用户名/仓库名.git  

* 添加文件到 本地仓库  

git add xxx
git add .(自动判断)  

* 提交文件  

git commit -m "说明信息"  
git push  

* Git不需重复输入账号和密码的方法  

vim创建文件 .git-credentials：  
输入：  
https://username:password@github.com  
执行：  
git config --global credential.helper store  

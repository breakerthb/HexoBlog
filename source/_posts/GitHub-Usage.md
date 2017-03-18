---
title: GitHub使用方法
date: 2017-03-18 08:31:00
tags:
    - git
categories: Tools
---

![常用命令](http://www.pythontab.com/uploadfile/2015/1224/20151224035849577.jpg)

# 1. 常用配置

## 1.1 设置Email

    $ git config --global user.name "your name"
    $ git config --global user.email "your_email@youremail.com"
    
查看结果：

    $ git config user.name
    $ git config user.email

## 1.2 用vim写日志

    $ git config --global core.editor vim

查看结果：

    $ cat ~/.git/config

# 2. 配置GitHub

## 2.1.首先在本地创建ssh key
    
    $ ssh-keygen -t rsa -C "your_email@youremail.com"
 
 之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在~/下生成.ssh文件夹，进去，打开id_rsa.pub，复制里面的key。

    $ cat ~/.ssh/id_rsa.pub
    
回到github，进入Account Settings，左边选择SSH Keys，Add SSH Key,title随便填，粘贴key。

## 2.2. 验证

为了验证是否成功，在git bash下输入：

    $ ssh -T git@github.com 

![验证](http://upload-images.jianshu.io/upload_images/145902-adc9c8f347b1c84b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果是第一次的会提示是否continue，输入yes就会看到：You’ve successfully authenticated, but GitHub does not provide shell access 。
这就表示已成功连上github。

![成功连接](http://img.my.csdn.net/uploads/201304/16/1366089042_1867.png)

## 2.3. 上传GitHub

接下来我们要做的就是把本地仓库传到github上去，在此之前还需要设置username和email，因为github每次commit都会记录他们。
	
	$ git config --global user.name "your name"
	$ git config --global user.email "your_email@youremail.com"
 
## 2.4. 添加远程地址

	$ git remote add origin git@github.com:yourName/yourRepo.git

 后面的yourName和yourRepo表示你再github的用户名和刚才新建的仓库，加完之后进入.git，打开config，这里会多出一个remote “origin”内容，这就是刚才添加的远程地址，也可以直接修改config来配置远程地址。

# 3. 提交、上传

## 3.1. 在本地仓库里添加文件
	
	$ git add a.txt
	$ git commit -m "first commit"
	
极简方式：

    $ git add .
    $ git commit -a
   
## 3.2. 上传到github：
  
	$ git push -u origin master

如果失败，可能是GitHub上的README.md不在本地，执行：

	$ git pull --rebase origin master # 合并
 
之后再执行push。

git push命令会将本地仓库推送到远程服务器。git pull命令则相反。

修改完代码后，使用git status可以查看文件的差别，使用git add 添加要commit的文件，也可以用git add -i来智能添加文件。之后git commit提交本次修改，git push上传到github。

# 4. 常用配置

## 4.1 添加fork me图标

![](https://s3.amazonaws.com/github/ribbons/forkme_left_red_aa0000.png)

ref : <https://github.com/blog/273-github-ribbons>

## Readme.md模板


    <a href="https://github.com/breakerthb/">
    <img style="position: absolute; top: 0; left: 0; border: 0;" src="https://camo.githubusercontent.com/82b228a3648bf44fc1163ef44c62fcc60081495e/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f6c6566745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_left_red_aa0000.png">
    </a>
    
    # Project Introduction
    
    XXXXX
    
    ![](https://github.com/breakerthb/AccessCount/blob/master/intro.png)
    
    # Flag
    
    - Test
    
    # Language
    
    - C++
    
    # Branch
    
    ## master
    
    A tool without UI
    
    ## ui
    
    A tool with UI
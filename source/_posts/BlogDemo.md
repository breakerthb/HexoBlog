---
title: 博客模板
date: 2017-03-11 17:00:50
tags:
	- blog
categories: None
---

# 1. 发布新文章

	$ hexo new "new article"
	
之后在source/_posts目录下面，多了一个new-article.md的文件。打开之后修改：

	---
	title: 新博客
	date: 2017-03-11 16:56:50
	tags:
		- blog
	categories: None
	---

修改后保存。之后执行并测试：

	$ hexo generate
	$ hexo server -p 8080
	
发布：

	$ hexo g // 重新生成博客
    $ hexo d // 发布
    
# 2. 图片

![天花板](http://breakerthb.github.io/PIC/avatar.png)

存储路径：source/PIC/avatar.png
显示路径：<http://breakerthb.github.io/PIC/avatar.png>





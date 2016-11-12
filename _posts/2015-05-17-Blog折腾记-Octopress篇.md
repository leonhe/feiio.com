---
layout: post
title: "Blog折腾记-Octopress篇"
date: 2015-05-17 19:34:09
categories: 技术工具
---

从wordpress到jekyll然后到hexo到octopress,也不知道在寻找什么？总是不停的折腾blog.也许这是最后一次折腾咯，也有可能这不会是最后一次。

总是在寻找最简洁和简单的工具和方式来表达自己，也许这一次找到咯octopress+markdown+github。
<!-- more-->
###安装步骤：

``` 
	//下载源码库
	git clone git://github.com/imathis/octopress.git octopress
	cd octopress

	gem install bundler
	bundle install

	rake install
```

### 配置文件

修改目录下的文件：_config.yml 

### 替换google字体文件CDN服务路径
国内你懂的禁用咯google fonts导致网站连接不上字体

修改source/_includes/custom/head.html和source/_includes/head.html里的

http://fonts.googleapis.com/ 到 http://fonts.useso.com/

### 基本命令
- 新建post 文章

```
	rake new_post["文章标题"]

```

- 新建页面 page

```
	rake new_page["页面名"]

```
- 生成静态文件和预览
```
	rake generate #生成静态文件
	rake watch #对有改动的观察
	rake preview #本地预览

```
- 发布
```
	rake deploy
```


### github设置
```
	rake setup_github_pages
```

### 设置域名
	
	在目录下soucres文件下创建一个CNAME的文件，并写进你的域名
	```
		echo 'your-domain.com' >> source/CNAME
		echo 'www.your-domain.com' >> source/CNAME
	```

基本安装的命令就如上所述,每次写完文章rake deploy一下多爽呀,让我们一起来享受像hacker一样写作吧!

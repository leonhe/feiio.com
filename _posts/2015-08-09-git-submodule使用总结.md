---
layout: post
title: "git submodule使用总结"
date: 2015-08-09 21:16:32
tags: git
categories: 技术工具
---

近一个星期公司项目由于需要使用git submodule来管理不同的子项目,自己对仅限了解add 、update、init进一步了解了对不同子项目代码的工作流作一个总结.
<!-- more -->
### Git Submodule相关命令

<pre>
 #增加子模块
 $ git submodule add  远程地址 本地存放目录

#浏览项目的子模块
$ git submodule

#对所有子模块进行相同的操作
$ git submodule foreach 命令
   比如: git submodule foreach git pull #这里是抓取所有子模块的最新版本
#初始化子模块
$ git submodule init
#对子模块进行更新操作
$ git submodule update
PS: 这个命令的时候需要确保子模块中没有修改
#切换所有子模块的分支
$ git submodule foreach git checkout -b 分支名
</pre>

### 修改子模块内容操作
对子模块内容修改一种方法是可以重新git clone 一份项目进行修改，然后在相对应的引用项目内update。另一种方法需要进入子模块的目录然后进行相应的git add & commit & push提交来确保更新到相应的子模块项目库中.这里需要注意一点就是如果子模块中做咯相应的修改在引用子模块的项目中也需要在此提交来更改引用项目对子模块进行引用的HAS记录值。



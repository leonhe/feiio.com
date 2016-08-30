title: "Emacs org-mode对表格的操作"
date: 2015-12-10 21:44:53
tags: Emacs
categories: 技术工具
---

最近在公司需要发布客户端组的每日的工作任务用了一段时间的excle来发布感觉使用也只能做个表格用用如果看历史纪录确实比较繁琐，想来一下是不是可以使用emacs的org-mode来做项目的纪录，并且还能支持远程同步到对应的服务器目录下和发布成pdf和html格式。这里也记下在org-mode中表格的常用操作作个记录：
<!-- more -->

### 常用快捷键：

<pre class="brush: bash;">
$ C-c #对当前表格对齐和格式化
$ M-S-left #删除当前列
$ M-left/right #对当前列进行左移或右移
$ M-S-up  #删除当前行
$ M-S-down #当前行的上新建一行
$ M-up/down #上下移动当前行
$ M-S-right #在当前列的左侧新建一列
$ Tab ＃切换光标到下一个表格中
$ S-Tab #切换光标到上一个表格中
$ C-c _ 或 |- #对当前行与行之间增加下划线
$ C-| #新建表格：行x列
$ C-c RET #当前行的下方插入一行
</pre>

### 相关资料
* [Org Mode Table](http://orgmode.org/org.html#Tables)
* [Org简明手册](http://www.cnblogs.com/Open_Source/archive/2011/07/17/2108747.html#sec-3)

title: "Flashbuilder4.7或Eclipse中修改自动注释的@author"
date: 2013-12-19 18:50:40
tags:
id: 50
categories:
  - 技术工具
---

在Flashbuilder中新建类加注释时，一直是系统自带的Administator要去修改很是麻烦.在网上搜了下修改方式如下：

1\. 打开Flash builder/Eclipse的安装目录
2.找到其中文件Flashbulder.ini/Eclipse.ini
3\. 在文件的最后一行加入<span style="color: #ff0000;">-Duser.name = 作者名</span>

重新启动应用程序(Flash builder/Eclipse).
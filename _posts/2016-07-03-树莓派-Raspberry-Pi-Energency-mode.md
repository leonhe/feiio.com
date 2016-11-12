---
layout: post
title: 树莓派(Raspberry Pi)提示Energency mode需要Root密码的处理方法
date: 2016-07-03 20:33:55
tags: [折腾,Raspberry PI ]
categories: 技术工具
---

闲置了一段时间的Raspberry Pi在上个周末问了电信要了路由器的超级管理用户名和密码后,打算架设一个外网能访问的个人云盘，最后把自己外接移动硬盘也接入上去,结果在其中mount硬盘的时候想在系统启动的时候自动挂载上去就在/etc/fstab文件中多加了两行数据,结果就导致重新启动系统后一直提示`Give me password for maintenance`需要输入Root密码进入Energency mode的提示,在多次入root密码后还是提示失败然后放弃了，另外找方法了.最后找到方法并解决掉了这个问题,具体处理办法和步骤如下：

<!-- more-->
1. 在启动时候按住shift键进入安装界面然后点击Edite Config
2. 选择command_file.txt tab,在其中rootwait前加上 `init=/etc/bin`
3. 重新启动系统,进入bash后,输入su
4. 输入：mount -p remount,rw /
5. vi /etc/fstab把自己加的那两行删除掉,然后重启.
6. 重复步骤1，2,删除其中的`init=/etc/bin`
7. 恭喜你进入系统
    
其实类似的问题在启动画面因为某些引导程序的配置文件失败，也可以通过上边的方法解决并正常启动系统.最后附上我启动失败提示的画面.

![Raspberry pi](/images/2016-7-3.jpg)

    
    

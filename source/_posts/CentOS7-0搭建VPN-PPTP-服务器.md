---
title: CentOS7.0搭建VPN(PPTP)服务器
date: 2016-05-14 17:34:55
tags: CentOS7
categories: 技术工具
---
一直用Shadowsocks的在PC端来进行翻墙,但发现最近手机有翻墙的需求就折腾了下对自己VPS进行PPTP的VPN搭建以下是搭建流程记录,VPS服务器操作系统是CentOS7.0.
## 安装组件
先更新下yum源,然后安装'ppp pptpd'两个组件
``` shell
yum update
yum install ppp pptpd
```
## 配置PPTP组件
- 编辑/etc/pptpd.conf,搜索localip和remoteip两行去掉前面的注释“#”
- 编辑/etc/ppp/options/pptpd,搜索"ms-dns"去掉两行前面的注释标识"#",然后修改DNS地址为"8.8.8.8和8.8.4.4"
- 编辑/etc/ppp/chap-secrets文件设置VPN的账号密码,在文件末尾增加一行"用户名 pptpd 密码 *"的格式设置.
- 修改内核参数/etc/sysctl.conf文件,在文件末尾增加"net.ipv4.ip_forward=1",然后运行"sysctl  -p"使内核生效.

## Firewalld配置

``` shell

firewall-cmd --permanent --direct --add-rule ipv4 filter INPUT 0 -i eth0 -p tcp --dport 1723 -j ACCEPT 
firewall-cmd --permanent --direct --add-rule ipv4 filter INPUT 0 -p gre -j ACCEPT 
firewall-cmd --permanent --direct --add-rule ipv4 filter POSTROUTING 0 -t nat -o eth0 -j MASQUERADE 
firewall-cmd --permanent --direct --add-rule ipv4 filter FORWARD 0 -i ppp+ -o eth0 -j ACCEPT 
firewall-cmd --permanent --direct --add-rule ipv4 filter FORWARD 0 -i eth0 -o ppp+ -j ACCEPT
firewall-cmd --reload

```

## PPTP服务操作
- 启动: `systemctl start pptpd`
- 重启: `systemctl restart pptpd`
- 增加服务: `systemctl enable service.pptpd`


## 参看资料
- [Centos 7搭建VPN（PPTP）服务器方法](http://www.wanghailin.cn/centos-7-vpn/)
- [Setup PPTP Server on CentOS](http://www.lightning-ashe.com/develop/setup-pptp-server-on-centos/)



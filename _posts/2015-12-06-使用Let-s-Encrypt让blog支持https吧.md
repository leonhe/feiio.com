---
layout: post
title: "对blog使用Let's Encrypt支持https"
date: 2015-12-06 11:45:59
tags: 折腾
categories: 技术工具
---

距上一篇时隔两月,看见同事的blog中支持了https自己也尝试的配置了下,也一起把blog的万网虚拟主机升级换上了高大上的[Vultr VPS](http://www.vultr.com/?ref=6862277)，线路选择的是洛杉矶机房。网站的证书使用的是[LET'S ENCRYPT](https://letsencrypt.org/)的免费证书但唯一缺点就是每隔90天需要重新的续签一次。
服务器系统是：CentOS 7.0
web服务器：Nginx。
<!-- more -->

以下就是对blog进行Let's Encrypt https支持的的配置的过程：

### Let's Encrypt的获取
<pre class="brush: bash; ">
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt
./letsencrypt-auto --nginx or apache
</pre>
NOTE: 第一次使用自动配置没有成功.

### 生成网站证书

<pre class="brush: bash; ">
//如果有多个域名请自行增加 *-d 域名* 参数
./letsencrypt-auto certonly --webroot -w 网站的主目录 -d 域名 
//查看生成证书的目录
$ls /etc/letsencrypt/live/域名/
</pre>

### nginx中的证书配置 

<pre clas="brush: bash; ">
server {

listen 443 ssl;
server_name  *.heyuanfei.cn *.feiio.com;
root        网站主目录
//SSL证书配置
ssl_certificate /etc/letsencrypt/live/heyuanfei.cn/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/heyuanfei.cn/privkey.pem;
ssl on;
}
//重新启动服务器
service nginx restart
</pre>

### 对HTTP请求的跳转配置

<pre class="brush: bash">
server {
listen       80 default_server;
listen       [::]:80 default_server;
server_name *.feiio.com *.heyuanfei.cn;
rewrite ^(.*)$ https://$host$1 permanent;
}
</pre>







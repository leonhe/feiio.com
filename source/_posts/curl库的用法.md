title: "libcurl库相关接口及其用法"
date: 2015-10-18 20:39:11
tags: [C++ , cocos2d-x]
categories: 编程技术
---

最近在做公司项目的时候需要在客户端中实现图片数据进行HTTP上传的一个需求,发现cocos2d-x中的HTTPClient实现起来甚是难用.最终使用curl库来实现相应的需求发现真是一切都是那么的美好,这里只记录下curl使用过程相对应的接口.   
<!--more-->
## Easy 和Multi的区别
* easy相关的接口是同步阻塞式的处理相对应的请求,优点在于它简单很快的实现
* Multi相关接口是异步处理,异步处理会及时对请求做出返回不会对线程进行阻塞.

## 使用流程
   在开始初始化全局的请求环境,然后通过curl_*_init获取相对应的handle,在初始化对应的请求头数据.然后 'curl_*_opt'设置相对应的返回回调函数和对应的网页头数据,curl_formadd接口设置对应请求所需要的数据.然后通过curl_perform对handle进行请求，在判断请求的结果，释放相关的请求数据和handle,如果不需要再次请求需要释放初始化的全局环境curl_globale_clean.

## 相关接口
* curl_global_init
  对curl请求环境进行初始化
* curl_easy_init()
  获取一个请求的handle
* curl_easy_cleanup()
  清理相应的请求handle
* curl_easy_setopt()
  设置相应的请求参数,不如请求的网页头,及请求的数据类型.及请求返回的相关回调函数.
* curl_easy_perform()
  对参数设置的handle进行一次请求,这个接口一般在请求头和请求数据,设置完成后调用。
* curl_easy_getinfo()
  获取handle的相关信息
* curl_formadd()
  设置请求的数据及其数据类型
* curl_formfree()
  释放请求数据指针
* curl_slist_append()
  设置请求体头数据
* curl_slist_free_all()
  释放请求的头数据
* curl_version()
  返回当前使用库相关版本的信息·
## 相关资料 
* [CURL and libcurl](http://curl.haxx.se/)



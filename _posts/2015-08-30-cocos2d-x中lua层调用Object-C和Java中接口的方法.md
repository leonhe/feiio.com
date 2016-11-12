---
layout: post
title: "cocos2d-x中lua层调用Object-C和Java中接口的方法"
date: 2015-08-30 20:00:41
tags: [cocos2d-x,Lua]
categories: [游戏开发]
---

在cocos2d-x lua框架生成项目目录src/cocos/cocos2d/luaj.lua和luaoc.lua两个文件，在项目中使用一个公共的lua文件通过device.platform进行对ios和android平台的管理进行相应的选择需要调用的接口.
在文件中分别提供了一个callStaticMethod的接口来对需要调用接口的调用.接口分为三个参数:类名,方法名,和参数.

在cocos2d-x lua项目中新建一个platformBridge.lua文件来对不同平台调用进行管理，lua文件代码如下：
<!-- more -->
<pre>
 function platformBridge()
   local result = {}
     if device.platform == "ios" then
        result.ios = import("cocos.cocos2d.luaoc")
	end
	if device.platform == "android" then
	    result.android = import("cocos.cocos2d.luaj")
	end
	return result
end

 --在其他文件中使用,这样就可以同个一个变量来对不同平台调用进行管理
 local bridgeLua = import("platformBridge")[device.platform]
 bridgeLua.callStaticMethod("类名","接口名","调用的参数")
</pre>

调用Object-C和Java接口cocos2d-x都是通过C++来做中转调用的.具体的实现细节参考cocos2d-x框架目录cocos/scripting/lua-bindings/manual/platform/中ios和anroid目录下的代码.

title: "ZeroBrane Studio自定义代码提示"
date: 2015-08-11 21:22:15
tags: [Lua, cocos2d-x]
categories: 技术工具
---


近来被cocos code ide来调试和开发cocos2d-x for lua项目搞的确实的比较蛋疼，真心不想在这里吐槽目前官方团队发布的版本是多么的难用,在前段时间实在忍无可忍的情况下在QQ群里问了一下最新的cocos code ide 什么时候发布得到的答案是9月份,在卡顿和各种系统下各种疑难杂症的情况下,不能让这样的工具来影响自己的开发效率,我也最终放弃使用cocos code ide改到比较老一点的ZeroBrane Studio下去开发脚本了,其实这个工具比较轻量级也不会卡顿,唯一的缺憾是不支持cocos2d-x api代码提示,也想了下直接通过官方的文档然后写个脚本支持来扩展对cocos2d-x api的支持.对学习zerobrane studio自定义代码提示做一个简短的记录.
<!--more-->
### 需要提示的API文件格式
需要提示api文件如下：

```
{
   {
   test = {
   			type="lib or class/function",
   			description="这里是描述",
   			args="(参数逗号分割)",
   			returns="(返回的类型)",
   			childs={ --这里是类型包含的子函数或值
   				bar = {
   					type="function",
   					description="这里是方法的描述",
   					valuetype="返回值的类型",
   					
   				}
   			},
   			inherits="父类型"
		
    
     }
   }
}

```


###api lua文件 table键值介绍：
对应自定义lua文件可使用table键描述
 * type: 类型值如下
 	* keyword: 需要描述语言的关键字符,如：do 、end
 	* class and lib: 一组包含方法和属性的类型 
 	* value: 描述值可以是一个常量
 	* function:接受参数的函数来描述(args)和返回值(返回)
 	* method:描述方法调用；方法类似于函数，也接受参数和返回值和值类型。唯一的区别是，method是在打":"时提示,而function是在打"."和":"后提示。
 * description: 方法或类的注释简短说明
 * args(可选): 字符串参数 如：”(file:file)“
 * returns(可选): 字符串，返回值类型,如："(boolean|nil)"
 * childs(可选):table类型,当前模块或类所包含的子类或方法
 * valuetype(可选):字符串，返回值的类型
 * inherits(可选): 字符串,当前模块继承的模块

 	
### 引用自定的API lua文件

在安装程序interpreters/目录下创建lua文件，如interpresters/Cocos2d.lua,然后在其中api健对应table值中增加"cocos2d":

```
	return {
		name="Cocos2d-x",
		description = "这里是一个cocos2d-x api",
		api={"cocos2d","baselib"},
		......
	}
```


### 参考资料
[Adding API and auto-complete description](http://studio.zerobrane.com/doc-api-auto-complete)

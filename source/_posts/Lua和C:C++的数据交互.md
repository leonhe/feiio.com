title: "Lua和C/C++的数据交互"
date: 2015-08-23 12:52:42
tags: [Lua ,C++]
categories: 编程技术
----

近日重新复习了下C\C++和Lua之间的数据交互知识,在这里也做一个简单的记录来作为以后的参考.

## Lua栈操作的介绍
Lua是一种嵌入式的脚本语言,不是独立运行的程序它可以通过链接对其他语言进行扩展，或让其他的语言来使用Lua的功能.其中包括使用lua作为库存在于C中或Lua作为主导权C作为一个库来使用.
<!--more-->
lua和C语言的交互都是在一个虚拟栈进行的,所有的API操作都是对栈上的数据进行交互.无论是C到Lua的操作还是Lua到C的操作.这里会存在两个问题
1. 动态类型和静态类型之间的区别，lua是动态性的语言，而C是静态语言
2. 自动内存管理和手动内存管理的区别.

lua没有采用C语言的联合来解决动态和静态的区别，其原因是不仅为C一门语言设计的保持Lua的宿主可以是任何一门语言。其次Lua是有垃圾回收机制的如果lua table保持在C变量中，Lua引擎则无法搜索出来回收.所以Lua会使用一个抽象的棧来对Lua和C之间的数据交互.

Lua的栈严格按照（LIFO）先出后进的规范来操作栈,栈中任何元素都保存着lua中的任何类型:
1. 获得lua中的一个值,调用Lua API函数,Lua会将指定的值压入栈，
2. 要将一个值传给Lua时则先要将这个值压入栈，调用Lua API函数,Lua则会把获取该值并将其从栈中弹出.

压栈操作会把第一个压入的元素索引标记为1，第二个为2，依次推至栈顶，使用负数来访问栈中元素，-1表示栈顶的元素.

##相关API

<pre>
压入栈:lua_push* (..)
检查元素类型: lua_is*(..)
栈中取出值: lua_to*()
检查栈是否有足够的空间:lua_checkstack(lua_State *l,int size);
返回栈中元素个数:lua_gettop(..)
将栈顶设置为一个指定位置: lua_settop(..)
栈中弹出指定个数的元素:lua_pop(..)
将索引上的值副本压入栈:lua_pushvalue(..)
删除栈中指定位置的元素:lua_remove(..)
移动指定元素空间开辟一个槽空间，将栈顶元素移到该位置: lua_insert(..)
移动栈顶的元素到指定位置:lua_replace(..)
*: string\nil\number\boolean\integer 
</pre>


##C中调用Lua
比如lua文件中有一个全局变量的table: conf={w=20,h=10}
<pre>

lua_stack* L = luaL_new_state();
luaL_openlibs(L);

lua_dofile("lua文件");

lua_getglobal(lua_stack*,"conf");
if(!lua_istable(lua_stack*,-1))
{
  printf("conf全局变量不是一个table")
}
//获取W的值
lua_pushstring(L,"w");
lua_gettable(L,-2)
if(!lua_isnumber(L,-1))
{
 printf("%s不是一个数字","w");
 }
 int w_val = lua_tonumber(L,-1);
 lua_pop(L,1);

//调用lua中方法
lua_getglobal(L,"方法名");
//这里可以压入参数值
lua_pushstring(L,"参数值")
//状态引用,参数个数,是否有返回值
if(lua_pcall(L,1,1,0)!=0)
{
  printf("函数调用失败 Error:%s",lua_tostring(L,-1));
}
//获取返回值
if(lua_isnumber(L,-1))
{
  int res_value = lua_tonumber(L,-1);
}
lua_close(L);

</pre>

##Lua调用C/C++
###注册C/C++函数到Lua中

<pre>
int mysin(lua_State* L)
{
//获取lua中的值
  int str = luaL_checknumber(L,1);
  printf("lua中传入参数的值:%d",str);
//获取第二个参数,调用lua方法
if(lua_isfunction(L,2))
{
   if(lua_pcall(L,0,0,0)!=0){
      printf("调用函数出错");
    }
}

//压入返回值到lua中
lua_pushstring(L,"这里是返回给lua的值");

return 1;
}

....

//压入函数到栈中
lua_pushcfunction(L,mysin);
lua_setglabal(L,"mysin");
....

lua中直接调用: local res=mysin(10,function()
   print("这里是参数函数")
end);


</pre>

### Lua调用C/C++中的自定义类型
<pre>
//自定类型绑定到lua中
struct MyType{
  int size;
}

static int mytype_new(lua_State* L)
{
MyType** mytype = (MyType**)lua_newuserdata(L,sizeof(MyType*));
*mytype=new MyType();
luaL_getmetatable(L,"feiio.MyType");
lua_setmetatable(L,-2);
   return 1;
}


static int mytype_getType(lua_State*L)
{
//这里获取创建的对象
MyType **mytype=(MyType**)luaL_checkudata(L,1,"feiio.MyType");
int size = (*mytype)->size;
lua_pushnumber(L,size);
 
 return 1;
 }

//Lua执行对象回收机制销毁对象时调用
static int mytype_gc(lua_State* L)
{
MyType** mytype = (MyType**)luaL_checkudata(L,1,"feiio.MyType");
if(mytype)
{
  delete mytype;
  }
  return 0;
}

int initMyType(lua_State* L)
{
static const luaL_Reg mylib[]={
{"new",mytype_new},
{NULL,NULL}
};

static const luaL_Reg myfunc[]={
{"getType",mytype_getType},
{"__gc",mytype_gc},
{NULL,NULL}
}
//实现面向对象的支持
luaL_newmetatable(L,"feiio.MyType");
lua_pushvalue(L,-1);
lua_setfield(L,-2,"__index");
lua_setfuncs(L,myfunc,0);
  luaL_newlib(L,mylib);


return 1;
  }

//main函数中
.....
initMyType(L);
luaL_requiref(L,"MyType",createType,1);
lua_settop(L,0)
.....

//Lua中调用
local mytype=MyType.new()
//面向对象的方式获取
mytype:getType()
</pre>

##相关资料
[使用C++模板来绑定Lua](http://lua-users.org/wiki/SimplerCppBinding)
[Lua教程:绑定一个简单的C++类(6)](http://zilongshanren.com/blog/2014-08-11-bind-a-simple-cpp-class-in-lua.html#%E7%BB%91%E5%AE%9Ac%E7%B1%BB)

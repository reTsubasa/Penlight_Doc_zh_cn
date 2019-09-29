# Penlight 1.6.0

[TOC]

# 目录

# 模块
##  pl.Set
### 概要

### 函数
#### Set(t)
创建一个set
*t* 可以是一个Set，Maple或List-like的表

#### **values (self)**

返回一个包含set内值的array-table

*self* 一个Set


### metamehtods

## pl.utils

常用工具

### 概要



### 函数

#### **quit (code, ...)**

通过调用`os.exit()`优雅退出

*code*：错误代码或消息，类型必须为string

*...*：用于消息格式的额外代码

- 因为quit本质是通过调用`utils.fprintf`函数打印到`io.stderr`，`...`的部分实质上是`fprintf`的参数。

- 调用`quit`后，`os.exit(code)`中，`code`会被置`-1`。
- 调用`quit`后,不会再执行之后的函数。

```lua
utils.quit("error happend:%s\n","somethings unusual!")
--output
error happend:somethings unusual!
```



#### **printf (fmt, ...)**

`string.format`的封装

*fmt*：格式

*...*：额外的参数

```lua
utils.printf("%s\n","hello, world!")
--output
hello, world!
```



#### **fprintf (f, fmt, ...)**

`string.format`的封装,并写入到指定文件

*f*：写入的文件句柄

*fmt*：格式

*...*：额外的参数

- 注意`f`是文件句柄，即需要提前获取
- 写入文件，依赖于句柄打开的参数。如果参数不正确，可能会写入失败。
- 不论写入是否成功，函数都不会有返回

```lua
--func3
local f = io.open("1.txt","a+")
utils.fprintf (f, "%s\n","hello, world!")
```

#### **import (t, T)**

将table注入到运行时(table)

*t*：需要注入的table

*T*：(可选)被注入的table，如果不提供则注入到`_G`

- 如果`t`是`string`类型时，函数行为变为require 模块`t`，并注入到`T`
- 不提供`T`时，会遍历`_G`确认是否已有与`t`中的key相同的值，目测效率很低(k/v遍历_G)
- 如果`t`和`T`存在相同key时，`T`的值不会被覆盖，但是会通过`fprintf()`输出一条警告信息到stderr，且无法关闭，同时函数自身不会返回错误状态和错误信息
- 因为上述各种奇怪的feature，有类似需求还是自己实现比较靠谱。

```lua
--func4
local t1 = {name1 = "t1",a = "text"}
local t2 = {name2 = "t2",a = "func"} 
utils.import(t1,t2)
for k,v in pairs(t2) do
  print(k,v)
end

--output
name1	t1
name2	t2
a	func
warning: '?.a' will not override existing symbol
```



#### **escape (s)**

转义任何`magic`字符为string

*s*：输入的字符

- 关于*magic*字符：[LUA Reference](http://www.lua.org/manual/5.1/manual.html#5.4.1)

```lua
local s = "%a%c"
utils.printf(utils.escape("%s\n"),"hello, world!")
--output
%s
```



#### **choose (cond, value1, value2)**

根据条件*cond*返回值，为真返回*value1*，反之返回*value2*

*cond*：条件

*value1*：*cond*为真时返回

*value2*：cond为假时返回(可选)

```lua
local a,b 
a,b = utils.choose(true,"a","b")
print(a,b)
a,b = utils.choose(nil,"a","b")
print(a,b)
a,b = utils.choose("a","b")
print(a,b)
```



#### **readfile (filename, is_bin)**

以字符形式返回文件内容

*filename*：文件路径

*is_bin*：二进制模式



**writefile (filename, str, is_bin)**

将字符写入文件

*filename*：文件路径

*str*：待写入的字符

*is_bin*：二进制模式

**返回**

1. true 或 nil
2. 错误信息

**报错**

如果*filename*或*str*不是字符时，则报错



#### **readlines (filename)**

读取文件，返回一个array table，其元素为文件每行的内容

*filename*：文件路径

**返回**

文件内容的一个table

**报错**

如果*filename*不是字符



#### split (s, re, plain, n)

根据分隔符切分字符，并返回切分后的array table

*s*：输入的字符

*re*：lua正则表达式，默认值为`%s+`

*plain*：不使用lua正则

*n*：(可选)最大切分的个数

**返回**

array table

**报错**

*s*不是字符时



#### **splitv (s, re)**

切分字符为几个值

*s*：输入的字符

*re*：分隔符，默认是空格

**返回**

切分数量的值

```lua
first,next = splitv('jane:doe',':')
```



#### **array_tostring (t, temp, tostr)**

将一个array table的值转换为str

*t*：一个array table

*temp*：(可选)缓冲区，不指定则自动分配

*tostr*：(可选)自定义转换函数，并以*t*的value,i作为入参传递。默认为tostring

**返回**

temp



- *tostr*函数不给定的场景下，函数使用`tostring`转换，没太多实际价值
- *temp*推荐是一个upvalue table，可以降低内存的分配

```lua
local function ts(v,i)
  return v+i
end
local tb = {1,2,3,4,5,6,}
local temp = {}
utils.array_tostring(tb,temp,ts)
print(json.encode(temp))

--output
[2,4,6,8,10,12]
```



#### **quote_arg (argument)**

引用一条命令的一个参数。引用的参数用于传递到`os.execute`, `pl.utils.execute` 或`pl.utils.executeex`.

*argument*：(string)参数

**返回**

引用的参数



#### **executeex (cmd, bin)**

执行一条shell命令，并返回其输出。本函数会重定向输出到临时文件并返回这些文件的内容

*cmd*：一个shell命令

*bin*：布尔值。如果为true，则以二进制模式读取临时文件

**返回**

1. true or nil，表示执行状态
2. 执行返回code
3. stdout 输出
4. 错误输出



#### **memoize (func)**

缓存*func*返回值，用于下次调用。方法特别适用于调用函数成本很高，但又很难预期其调用时间

*func*：至少包含一个参数的函数



**返回**

至少包含一个参数函数，该参数将被视作key




## 类

## 手册

## 例子


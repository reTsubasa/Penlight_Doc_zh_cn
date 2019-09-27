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




## 类

## 手册

## 例子


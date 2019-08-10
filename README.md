# Penlight库中文文档



## 为什么使用Penlight

Penlight 提供了一套以纯lua实现的工具集。关注于提供数据输入(比如读取配置文件)，实用函数(如map，reduce函数)，以及系统路径参数的控制。这些工具为lua的使用提供了极大的便利。

## 关于中文文档

- 目前提供

  **Working** ：[1.6.0](https://github.com/reTsubasa/Penlight_Doc_zh_cn/tree/master/1.6.0)

  *挖坑但不知道什么时候能填完*  ~>_<~

  其他版本可以前往【[Penlight repo](https://github.com/stevedonovan/Penlight)】查看。

- 不同于官方文档通过【[Ldoc](https://github.com/stevedonovan/LDoc)】库生成，我希望翻译成为lua官方手册那样的一页格式，以便于速查速用。同时因为中文习惯上的原因，应该会大幅调整原有文档的编排。

- 可能会添加模块方法的使用的一些例子，便于更容易的理解。

- 应该会有些翻译问题或自身理解问题，不太好理解的地方，建议参照原作者文档。



## 模块概览

### 路径, 文件和目录

  * `path`: 提供路径操作及文件操作
  * `dir`: 目录的查询/创建/删除操作
  * `file`: 提供文件读、写、移动、复制操作

### 应用支持

  * `app`: 应用支持函数，提供诸如函数调用路径控制，简单的参数解析等功能
  * `lapp`: 应用参数解析库，支持类似模板渲染式的使用
  * `config`: 解析配置文件
  * `strict`: 未使用的全局变量检查
  * `utils`,`compat`:常用工具集
  * `types`: lua原生type函数的能力扩展

### 增强的字符操作

  * `stringx`: string函数的扩展，类似Python`string`库的方式
  * `stringio`:  以文本对象的形式读写string
  * `lexer`:  词法扫描器，用于从文本创建一系列标记
  * `text`:  文版操作工具
  * `template`:  模板工具
  * `sip`:  简单输入模式(Simple Input Patterns (SIP))

### 增强的表操作

  * `tablex`: 原生table的增强
  * `pretty`: table的美化输出及更安全的操作工具
  * `List`: Python风格的列表实现
  * `Map`, `Set`, `OrderedMap`: 常见类的封装
  * `data`: 表格的查询工具
  * `array2d`: 2维数组的操作工具
  * `permute`: 通用的置换工具

### 迭代，面先对象及实用函数

   * `seq`:  提供序列特性(sequences)的迭代器工具
   * `class`: 通用类工具
   * `func`: 常用使用函数
   * `comprehension`: 列表功能扩展

## 安装

- 使用[LuaRocks](https://luarocks.org): 

  执行 `luarocks install penlight`

- 手动

  复制 `lua/pl` 文件夹到lua库目录下.

  Linux下通常位于：`/usr/local/share/lua/5.x` 

  Windows则在 `C:\Program Files\Lua\5.x\lua`

## 依赖

文件和目录操作依赖于 [LuaFileSystem](https://keplerproject.github.io/luafilesystem/)

如果需要在Windows下，优雅的使用 `dir.copyfile` 还需要安装 [Alien](http://mascarenhas.github.io/alien/)

这2个库均提供Windows的版本
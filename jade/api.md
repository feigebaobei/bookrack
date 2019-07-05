# API

[TOC]

这页内容是如何在node.js中使用js api

## 安装

`npm i jade`

## 用法

所有的api方法都会用到这些options.下列是作为options对象的各个属性。
|属性|说明|
|-|-|
|filename|文件名|
|doctype|若模板没有指定doctype.你就可以使用该属性指定doctype.|
|pretty|是否格式化。|
|self|是否使用命名空间控制local（默认为false）|
|debug|若为true，则标记输出。|
|compileDebug|若设置为true。编译后的文件会包含更好的错误信息。|
|cache|若为true，则缓存编译方法。filename必须设置。|
|compiler|不使用默认的编译器|
|parser|不使用默认的解析器|
|globals|定义在模板中可使用的全局变量|

### jade.compile(source, options)

返回编译后的html内容。

### jade.compileFile(path, options)

把指定路径上的jade文件编辑后返回html内容。

### jade.compileClient(source, options)

在客户端运行时把jade文件编译为html内容，并返回一个代表这个方法的js string.

### jade.compileFileClient(path, options)



### jade.render(source, options)

把jade文件渲染为html。

### jade.renderFile(path, options)

把jade文件渲染为html。


# usage

## introduce

我们一旦安装flow，我们就会想如何使用它。几乎对于所有新flow项目，我们都会用到以下模式。  

- `flow init` 用来初始化项目。  
- `flow` 开始使用flow后台处理。  
- `// @flow` 标明哪个文件需要flow监控。(下方称为flow file(flow文件))  
- 编写需要flow的代码。  
- 查检代码中的类型错误。  

## initialize we project

为一个项目准备响应flow请求只需要一行代码。  
`flow init`  
在项目的根目录的命令行中运行这行代码，会在根目录里创建一个`.flowconfig`的文件。在这个文件里会告诉flow后台程序开始检查这个根目录下的所以需要检查的文件里的类型错误。  
若我们按部就班地做以上准备工作，那么现在我们就可以使用flow了。  
> 通常会创建一个空的`.flowconfig`文件。但是我们有多种方法自定义flow配置项。  

## run the flow background process

flow的核心功能是可以快速检查代码中的类型错误。一旦flow可以检查项目，它就会立即并快速地检查我的代码。  
第一次开始flow的后台程序去检查flow文件时使用。  
`flow status`  
后台程序会在flow文件有改变时立即检查错误。  
> 使用`flow`与使用`flow status`的效果相同。

> 在任何给定时间只会运行一个后台进程，因此如果多次运行flow，它将使用相同的进程。  

> 需要停止时执行`flow stop`  

## prepare we code for flow

后台程序会监控所有的flow file。现在有一个问题，如何让flow知道应该监控哪些文件？在需要监控的文件的头部添加`// @flow`或`/* @flow */`。flow的后台程序会收集所有的有这个标志的文件并且检查错误。  

> flow的后台程序通常只会在处理有标志的文件，没有标志的文件会被忽略。除非使用`flow check --all`,这行命令的作用范围包括基本用法的作用范围。  

## write flow code

现在已经完成安装和初始化。我们可以开始写实际的flow代码了。现在我们拥有所有已经标记的文件的所有检查类型的权限。下面是一个flow file的例子。  
```
// @flow
function foo(x: ?number): string {
    if (x) {
        return x
    }
    return "default string"
}
```
在function的参数的后面写上期望该function返回的数据类型。  

## check we code

现在是一个伟大的时刻，flow会实时检查项目中的代码。并且指出错误。在命令行中运行`flow`或`flow status`  


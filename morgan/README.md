# Introduction

## morgan

用于记录http请求的node.js的模块。

## 使用方法

`morgan(format[, options])`  
format 记录日志的格式（String型）。
options 是配置项组成的对象。

有4种使用形式

morgan('tiny') // 使用内置format的名称
morgan(':method :url') // 使用自定义的formatString
morgan((tokens, req, res) => {...}) // 使用返回formatString的方法。tokens是包含所以已定义的全部token的对象。我通过log得到token就是morgan的实例。
morgan(morgan.compile(formatString)) // 使用预编译后的formatFunction.

## api

    var morgan = require('morgan')
    morgan(format, options)
    // format 是String/Function。morgan内置了一些记录格式的名称。也可以使用自定义的格式名称。若format是Function时，接收3个参数（tokens, req, res）这个方法返回一个string作为记录格式，当返回undefined/null时跳过记录。
    // options 是配置项组成的对象。
    
    morgan('tiny') // 使用内置的格式
    morgan(':method :url :status :res[content-length] - :response-time ms') // 使用预定义的格式
    morgan((tokens, req, res) => [tokens.method(req, res), tokens.url(req, res), tokens.status(req, res), tokens.res(req, res, 'content-length'), '-', tokens['response-time'](req, res), 'ms'].join(' ')) // 使用自定义的格式
    
    morgan接收一些配置项作为options对象。

### format

#### morgan有一些内置的format。

| option   | type | describe                                                     | default                                                      | demo |
| -------- | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| combined |      | 使用标准apache的行内输出                                     | :remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length] ":referrer" ":user-agent" |      |
| common   |      | 使用标准apache的一般输出                                     | :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length] |      |
| dev      |      | 简明地输出有颜色的状态码的输出。服务端出错时，使用红色。客户端出错时，使用黄色。重定向时，使用蓝绿色。别的没有颜色。 | :method :url :status :response-time ms - :res[content-length] |      |
| short    |      | 使用比默认更短的输出。                                       | :remote-addr :remote-user :method :url HTTP/:http-version :status :res[content-length] - :response-time ms |      |
| tiny     |      | 使用最小的输出                                               | :method :url :status :res[content-length] - :response-time ms |      |

#### 自定义format。

morgan.format('formatName', 'format') // 定义format
formatName 是format的名称。
format 是format的格式。string型。里面可以使用token。
morgan.format('joke', `[joke] :method`) // demo
morgan('formatName') // 使用

在morgan里的源码是这样的。

```
function format (name, fn) {
    morgan[name] = fn
    return this
}
morgan.format = format
```

### tokens

下面是内置的token类型

| tokenName              | describe                                                     |      |
| ---------------------- | ------------------------------------------------------------ | ---- |
| :data[format]          | 使用哪种时间格式（可选的有：`clf``iso``web`）                |      |
| :http-version          |                                                              |      |
| :method                |                                                              |      |
| :referrer              |                                                              |      |
| :remote-addr           |                                                              |      |
| :remote-user           |                                                              |      |
| :req[header]           | 请求头里的header.若没有则返回`-`                             |      |
| :res[header]           |                                                              |      |
| :response-time[digits] | 从接到请求到返回数据之间花费的时间。digits是精度。默认是3.表示ms. |      |
| :status                |                                                              |      |
| :url                   |                                                              |      |
| :user-agent            |                                                              |      |

创建新的token
`morgan.token('tokenName', cb)` // cb是接受req,res的回调方法。
`morgan.token('type', (req, res) => req.headers[content-type])` // 定义
`:type`// 使用

在morgan里的源码是这样的。

```
function token (name, fn) {
    morgan[name] = fn
    return this
}
morgan.token = token
```

由tokens组成format再作为morgan的参数使用。

### options

| option    | type                           | describe                                    | default | demo |
| --------- | ------------------------------ | ------------------------------------------- | ------- | ---- |
| immediate | Boolean                        | true:收到请求时记录。false:返回请求时记录。 |         |      |
| skip      | Boolean / Function(req, res){} | 是否跳过记录                                |         |      |
| stream    |                                | 记录的输出流配置。默认使用`process.stdout`  |         |      |

### morgan.compile(format) 

把string型的format编译为function型的format。
这个function接收三个参数（tokens, req, res）。tokens是一个具有所有已经定义的token的对象，req是请求，res是回馈。该方法返回一个string型的单行记录文本。若返回`undefined ，/ null`，则跳过记录。

format 是String型。使用单行文本，里面写入`:token[params]`.

使用format时一般是`morgan.format(name, format)`，但是还有一些高级用法可以直接使用`compile`。如：`morgan.compile('[joke] :method :url :response-time ms')`。这种方法是把formatString预编译为formatFunction.

`morgan.compile(formatString)`返回的是`Function`，所以使用方法如下：

```
morgan.compile('[joke] :method')
// or
let ff = morgan.compile('[joke] :method')
morgan.format('fs', ff)
morgan('fs')
```
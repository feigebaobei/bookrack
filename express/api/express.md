# express

创建一个express应用。`express()`方法是顶级方法。通过`express`模块输出。

```
var express = require('express')
var app = express()
```

[TOC]



## Methods

### express.json([options])

> express v4.16.0之前的版本可以使用这个中间件。

它是一个内置在express的中间件。它是基于[body-parser](http://www.expressjs.com.cn/en/resources/middleware/body-parser.html)可以解析使用json格式发来的请求。

返回的中间件只能解析json，并且只有查看头部的`Content-Type`的属性值与`options`的type属性是否一致。这个解析器可以接受任意编码格式的body，并且支持自动解压gzip/defalte。

在中间件（即：`req.body`）后面的请求对象的填充一个包含已经解析数据的新的body对象。或者一个空对象 。若没有body对象可解析，则`Content-Type`不会被匹配或出现错误。

> 因`req.body`是基于用户输入形成的，所以是不可相信的。需要经过验证才可信任。如：`req.body.foo.toString()`可能有多次错误。比如foo可能不是string，或toString不是方法等。

下面的表格是`options`对象的属性及说明。

| 属性    | 说明                                                         | 类型     | 默认               |
| ------- | ------------------------------------------------------------ | -------- | ------------------ |
| inflate | 是否可以压缩body.                                            | Boolean  | true               |
| limit   | 控制request body的最大值。若是number，则单位是`b`若是string，则使用[bytes](https://www.npmjs.com/package/bytes)解析。 | Mixed    | '100kb'            |
| reviver | 把`reviver`作为`JSON.parse`的第二个参数。                    | Function | null               |
| strict  | 是否仅接受Array或Object对象，若不接受，则接受`JSON.parse`接受的数据。 | Boolean  | true               |
| type    | 定义中间件解析哪种媒体类型。其值可以是sting/字符串组成的数组/function。若不是function，其值直接给[type-is]()库处理，它可以是扩展名（如json）mime类型（如application/json）或带有通配符的mime类型（如`*/*`或`*/json`）。菲是function，则调用fn(req)，若返回真，则解析。 | Mixed    | 'application/json' |
| verify  | 若使用这个属性，则执行`verify(req, res, buf, encoding)`.buf是request body的Buffer. encoding是request的编码。当抛出error时中止。 | Function | undefined          |

### express.static(root, [options])

这是express内置的方法。它使用[serve-static](http://www.expressjs.com.cn/en/resources/middleware/serve-static.html)用来提供静态文件服务。

> 有更好的方法。使用[revser proxy](http://www.expressjs.com.cn/en/advanced/best-practice-performance.html#use-a-reverse-proxy)缓存可以提高提供静态文件的服务性能。

root参数是项目的相对根目录的提供静态资源的目录。这个方法结合`req.url`和根目录来提供静态服务文件。当文件找不到时，不会返回404，会执行`next()`方法，执行后面的中间件。它允许堆栈和回退。

下面是`options`对象的属性。还有一个[例子](http://expressjs.com/en/4x/api.html#example.of.express.static)

| 属性     | 说明                                                 | 类型   | 默认     |
| -------- | ---------------------------------------------------- | ------ | -------- |
| dotfiles | 定义如何对待点文件（以点开头的文件或目录，如.git）。 | String | 'ignore' |
| etag     |                                                      |        |          |
|          |                                                      |        |          |
|          |                                                      |        |          |
|          |                                                      |        |          |
|          |                                                      |        |          |
|          |                                                      |        |          |
|          |                                                      |        |          |
|          |                                                      |        |          |
|          |                                                      |        |          |



### express.Router([options])

### express.urlencoded([options])


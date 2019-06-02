# Guide

## Routing

应用是如何根据客户端的请求做出回应的。这里有一个有介绍路由的[基本教程](http://expressjs.com/en/starter/basic-routing.html)

你使用路由的方法与使用http的方法一样。如：`app.get()` 是get请求。`app.post()`是post请求。这里有[[app.METHOD]](http://expressjs.com/en/4x/api.html#app.METHOD)的全部方法。你可以看到`app.all()`控制所有http方法。也可以看到`app.use()`明确[中间件](http://expressjs.com/en/guide/using-middleware.html)做为回调函数。

特定的路由请求方式只能响应特定的路由请求方法和http请求方式。当二者的请求方式匹配时才能执行回调函数。

实际上。路由的一个方式可以对应多个回调函数，当使用多个回调函数时最重要的是提供`next`参数。当执行`next()`时会停止执行当前方法，并开始执行下一个回调函数。

下面是一个非常基础的例子：

```
var express = require('express')
var app = express()
app.get('/', function (req, res) {
	res.send('hello world')
})
```

### Route methods

路由方式可以察觉http的请求方式，并且匹配express类的实例。

以下是一个定义了app的根路径的get/post方式的路由。

```
app.get('/', (req, res) => {
	res.send('Get')
})
app.post('/', (req, res) => {
	res.send('Post')
})
```

express支持匹配所有的http请求方法。

有一个特殊的路由方式——`app.all()`，作用于当前路由的全部请求方式，一般用于加载中间件方法。以下是一个控制`/secret`路由的全部请求方式。它支持所有http请求方式。

```
app.all('/secret'， (req, res, next) => {
	console.log('all')
	next() // pass control to the next handler
})
```

### Route paths

使用路由的路径与路由的请求方式结合来确定请求可以被访问。路由路径可以是字符串、字符串模式、与字符串匹配的表达式。

`?+*()`它们是正则表达式的子元素。`-.`它们会被逐字理解。

如果你需要使用`$`做为路由字符串，可以使用`([`和`])`围绕`$`，使其逃逸正则表达式。如：你需要定义的路由路径是`'/data/$book'`，需要写成`/data/([\$])book`

> express可以使用`path-to-regexp`匹配路由路径。可以在其[文档](https://www.npmjs.com/package/path-to-regexp)学习如何定义路由路径。
>
> [Express Route Tester](http://forbeslindesay.github.io/express-route-tester/)是一个测试基本路由路径的工具。但是它不支持模式匹配。
>
> 查询字符串不是路由路径。

下面是一些基本的路由路径的例子。

```
app.get('/', (req, res) => {...}) // 匹配根路由的get方式
app.get('/about', (req, res) => {...}) // 匹配/about的get方式
app.get('/random.text', (req, res) => {...}) // 匹配random.text路径的get方式
app.get('/ab?cd', (req, res) => {...}) // 可以匹配 abcd / acd
app.get('/ab+cd', (req, res) => {...}) // abcd abbcd abbbcd ...
app.get('/ab*cd', (req, res) => {...}) // abrandomcd
app.get('/ab(cd)?e', (req, res) => {...}) // abcde abe
app.get('/a/', (req, res) => {...}) // 所有包含a的路由路径
app.get('/.*fly$/', (req, res) => {...}) // randomfly
```

#### Route parameters

路由参数是在指定url的位置上的url碎片。通常使用`req.params`对象结合路由参数的name在url中得到数据。

```
Route path: /users/:userId/books/:bookId
Request URL: http://localhost:3000/users/34/books/8989
req.params: {userId: '34', bookId: 8989}
```

在路由中设置路由参数就是在指定参数前使用`:`。

```
app.get('/users/:userId/books/:bookId', (req, res) => {
	res.send(req.params)
})
```

路由参数必须是英文字母或数字（[A-Za-z0-9]）。

`-.`是被逐字翻译的。（路由参数中不能使用它们）。利用这个特性我们可以这么做。

```
Route path: /flights/:from-:to
Request URL: http://localhost:3000/flights/lax-sfo
req.params: {from: 'lax', to: 'sfo'}
```

> 因为正则表达式经常使用在路由参数上，从安全角度考虑应使用`\`转义。如`\\d+`
>
> 在express4.x `*`不会转义。可以使用`{0,}`代替。在express 5后会被确定下来。

### Route handlers

你可以使用多个回调函数处理请求，就像中间件一个。只有一个例外，这些回调函数可能调用`next('router')`跳过剩下的回调函数。利用这个原理，我们可以在某个路由上添加一些前提条件，然后通过控制剩下的路由

路由控制器可以是单位函数，也可以是函数组成的数组，也可以是二者结合使用。以下是例子。

```
// use single fn
app.get('/', (req, res) => {...})
// use more than one fn
app.get('/', (req, res, next) => {
	...
	next()
}, (req, res, next) => {
	...
	next()
}, (req, res) => {
	// 最后一个fn可以不写next
	...
})
// use array
app.get('/', [fn0, fn1, fn2])
// use array and fn
app.get('/', [fn0, fn1], fn2)
// 记得串联使用fn时使用next()
```

### Response methods

下表中的response对象的方法可以给客户端发送response，并结束request-response环。若路由没有请求方式，则挂起这次请求。

| 请求方式         | 说明                                                       |
| ---------------- | ---------------------------------------------------------- |
| res.download()   | 提示下载文件。                                             |
| res.end()        | 结束响应处理。                                             |
| res.json()       | 发送json回馈。                                             |
| res.jsonp()      | 发送jsonp回馈。                                            |
| res.redirect()   | 重定向请求。                                               |
| res.render()     | 渲染视图模板。                                             |
| res.send()       | 发送各种类型的回馈。                                       |
| res.sendFile()   | 将文件作为8位字节流发送。                                  |
| res.sendStatus() | 设置回馈状态码并发送并将其字符串表示形式作为响应主体发送。 |

### app.route()

可以使用`app.route()`创建链式调用路由操作者。因为对于一个明确的路由路径创建一个模块化的路由是很有用的，还可以减少冗余代码，减少拼写错误。更多信息可以查看[Router()文档](http://www.expressjs.com.cn/en/4x/api.html#router)。

下面是一个链式调用路由操作者的例子。

```
app.route('/')
	.get((req, res) => {
		res.send('get')
	})
	.post((req, res) => {
		res.send('post')
	})
	.put((req, res) => {
		res.send('put')
	})
	.delete((req, res) => {
		res.send('delete')
	})
```

### express.Router

使用`express.Router`类创建模块化，可挂载的路由控制器。一个路由实例就是一个完整的中间件和路由系统。

下面是创建了一个模块化的路由，它加载了一个中间件函数，定义了一个路由，并在入口文件挂载了它。

```
// 在入口文件中引入、挂载birds.js
var birds = require('./birds')
app.use('/birds', birds)
// brids.js
var express = require('express')
var router = express.Router()
// 在这个路由明确使用中间件。
router.use(function timelog (req, res, next) {
	//
	next()
})
router.get('/', (req, res) => {
	//
})
router.gete('/about', (req, res) => {
	//
})
module.exports = router
```

这个应用现在可以请求`/birds`和`/birds/about`，以及可以触发明确绑定在这个路由上的`timeout`中间件。


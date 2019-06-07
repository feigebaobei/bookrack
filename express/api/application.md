# Application

[TOC]

通常使用`app`表示express应用。使用`express()`方法实例化。

```
var express = require('express'),
  app = express()
app.get('', (req, res) => {
  res.send('hello world')
})
app.listen(3000)
```

这个app对象有以下方法。

- 路由的http请求。例子请查看[app.METHOD](http://expressjs.com/en/4x/api.html#app.METHOD)和[app.param](http://expressjs.com/en/4x/api.html#app.param).
- 配置中间件。[app.route](http://expressjs.com/en/4x/api.html#app.route)
- 渲染html视图。[app.render](http://expressjs.com/en/4x/api.html#app.render)
- 注册模板引擎。[app.engine](http://expressjs.com/en/4x/api.html#app.engine)

还可以设置一些应用的其它属性，更多信息可以查看[Application settings](http://expressjs.com/en/4x/api.html#app.settings.table)

> express应用对象可以参考[request对象](<http://expressjs.com/en/4x/api.html#req>)和[response对象](<http://expressjs.com/en/4x/api.html#res>)作为`req.app`和`rea.app`。

## Properties

### app.locals

app.locals的属性是应用程序的局部变量。

```
console.dir(app.locals.title) // 'my app'
console.dir(app.locals.email) // me@myapp.com
```

app.locals属性在整个应用程序的生命周期内有效。[res.locals](<http://expressjs.com/en/4x/api.html#res.locals>)只在request的生命周期内有效。

你可以取得应用中的模板渲染中的局部变量。为模板提供这个变量很有用，就像应用级的数据一样。使用`req.app.locals`（见[req.app](<http://expressjs.com/en/4x/api.html#req.app>)）可以中间件的得到局部变量。

```
app.locals.title = 'my app'
app.locals.strftime = require('strftime')
app.locals.email = 'my@myapp.com'
```

### app.mountpath

app.mountpath可以包括一个或多个子应用程序的路径模式。

> 子应用是一个express的实例。它可以控制一个路由上的请求。

```
var expres = require('express'),
  app = express(),
  admin = expres()
admin.get('/', (req, res) => {
  res.send('admin home page')
})
app.use('/admin', admin)
```

它的作用与req对象的`baseUrl`很像。建议不要使用要req.baseUrl代替url路径。

若一个子应用挂载到多个路由路径上时，`app.mounthpath`会返回这些路由路径模式。以下是一个例子。

```
var admin = express()
admin.get('/', (req, res) => {
  console.log(admin.mountpath) // ['/adm*n', '/manager']
  res.send('String')
})
var secret = express()
secret.get('/', (req, res) => {
  console.log(secret.mountpath) // /secr*t
  res.send('String')
})
admin.use('/secr*t', secret)
admin.use(['/adm*n', '/manager'], admin)
```

## Events

### app.on('mount', callback(parent))

当子应用挂载到父应用上时触发`mount`事件。此时这个父应用将传递给回调函数。

> 子应用将不继承被设置的默认值。你必须在子应用中再设置一次。
>
> 继承的设置没有默认值。
>
> 更多信息请查看[Application settings](<http://expressjs.com/en/4x/api.html#app.settings.table>)

```
var admin = express()
admin.on('mount', parent => {
  console.log('admin mounted')
  console.log(parent)
})
admin.get('/', (req, res) => {
  res.send('admin home page')
})
app.use('/admin', admin)
```

## Methods

### app.all(path, callback [, callback...])

这个方法类似标准的[app.METHOD()](<http://expressjs.com/en/4x/api.html#app.METHOD>)方法，它可以接受所有动作的http请求。

| 参数     | 说明                                                         | 默认值 |
| -------- | ------------------------------------------------------------ | ------ |
| path     | 当路由被匹配是触发相应的中间件函数。其可以是以下任一情况。- 一个string。 - 一个路径模式。 - 一个正则表达式 - 由以上三个情况组成的数组。[例子](<http://expressjs.com/en/4x/api.html#path-examples>) | '/'    |
| callback | 回调函数可以做以下事：- 一个中间件方法。 - 一些中间件方法。 - 由方法组成的中间件数组。 - 以上三个情况的组合。你可以使用多个中间件方法。它们使用`next()`触发以后的中间件方法。可以使用这个原理为路由设置预处理条件。[中间件的例子](<http://expressjs.com/en/4x/api.html#middleware-callback-function-examples>) | None   |

下面的中间件函数是当请求/secret路由的任一http请求参数都会执行。

```
app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...')
  next() // pass control to the next handler
})
```

这个方法是一个非常有用逻辑一特殊的任一的配置项。如果你定义 下面的路由在顶部。它会匹配所有的请求路由，在验证、加载user时。为满足以上条件，我们可以使用预置任务。当执行`next()`时触发下一个方法。

`app.all('*', requireAuthentication, loadUser)` <=> `app.all('*', requireAuthentication) app.all('*', loaduser)`

另一个例子：全局白名单方法，这个例子与前一个很像。但是严格限制使用'/api'开头。

`app.all('/api/*', requireAuthentication)`

### app.delete(path, callback [, callback...])

为指定的路由绑定delete请求触发指定的中间件（们）。更多信息请查看[routing guide](<http://expressjs.com/en/guide/routing.html>)

参数说明

| 参数     | 说明                   | 默认值 |
| -------- | ---------------------- | ------ |
| path     | 当路由被匹配是触发相应的中间件函数。其可以是以下任一情况。- 一个string。 - 一个路径模式。 - 一个正则表达式 - 由以上三个情况组成的数组。[例子](<http://expressjs.com/en/4x/api.html#path-examples>) | '/'    |
| callback | 回调函数可以做以下事：- 一个中间件方法。 - 一些中间件方法。 - 由方法组成的中间件数组。 - 以上三个情况的组合。你可以使用多个中间件方法。它们使用`next()`触发以后的中间件方法。可以使用这个原理为路由设置预处理条件。[中间件的例子](<http://expressjs.com/en/4x/api.html#middleware-callback-function-examples>) | None   |

```
app.delete('/', (req, res) => {
  res.send('delete home page')
})
```

### app.disable(name)

设置name的值为false。这个name是[app settings table](<http://expressjs.com/en/4x/api.html#app.settings.table>)里的一项。执行`app.set('foo', false)`与执行`app.disable('foo')`一样。

`app.disable('trust proxy')` <=> `app.get('trust proxy')` 都是false.

### app.disabled(name)

当app setting table里的name值为false时返回true。`app.set('foo', true)`与`app.enable('foo')`作用一样。

`app.enable('trust proxy')` <=> `app.disabled('trust proxy')` 都是fasle

### app.enable(name)

### app.enabled(name)

### app.engine(ext, callback)

为给定的ext文件使用指定的模板引擎。

默认情况下，express根据文件扩展名请求。如你渲染'foo.pug'文件。express会在内部调用以下内容，并缓存require()在后续中提高性能。

`app.engine('pug', require('pug').__express)`

对于没有提供开箱即用的`__express`方法，或你想为不同的扩展名使用不同的模板引擎，可以使用它。

下面的例子是为渲染ejs文件指定渲染引擎的。

`app.engine('html', require('ejs').renderFile)`

// 这段我不会。   这时，ejs提供了`.renderFile()`方法。

一些模板引擎不遵从一些惯例。[consolidate.js](<https://github.com/tj/consolidate.js>)库映射node模板引擎到惯例。所以可以无缝对接express.

```
var engines = require('consolidate')
app.engine('haml', engines.haml)
app.engine('html', engines.hogan)
```

### app.get(name)

返回app settings table里的name的值。

```
app.get('title')// undefined
app.set('title', 'my site')
app.get('title') // my site
```

### app.get(path, callback[, callback...])

为指定路由路径使用指定的回调函数（们）。

### app.listen(path, [callback])

开发unix socket并监听给定的路径。这个方法与node 的 http.Server.listen() 作用一样。

```
var express = require('express')
var app = express()
app.listen('/tmp/sock')
```

### `app.listen([port[, host[, backlog]]][, callback])`

为host/port绑定监听链接。这个方法与`http.Server.listen()`一致。  

若省略port或为0,则系统会分配一个未使用的port。这个情况对于自动任务（如：测试）很有用。

```
var express = require('express'),
  app = express()
app.listen(3000)
```

这个`app`是由`express()`返回的。用来处理node的http服务的请求。它很容易提供http/https服务并使用相同的代码。

```
var express = require('express'),
  https = require('https'),
  http = require('http'),
  app = express()
http.createServer(app).listen(80)
https.createServer(options, app).listen(443)
```

`app.listen()`方法返回一个`http.Server`对象。这种方式是一个惯例。

```
app.listen = function () {
	var server = http.createServer(this)
	return server.listen.apply(server, arguments)
}
```

### app.METHOD(path, callback[, callback ...])

路由是一个http请求，它的请求方式就是http请求方式。如GET,PUT,POST……，在路由里都是使用小写。因此结果为`app.get()`,`app.post()`……

express支持的路由方式与http的路由方式一样，使用相同的名字。

- checkout
- copy
- delete
- get
- head
- lock
- merge
- mkactivity
- mkcol
- move
- m-search
- notify
- options
- patch
- post
- purge
- put
- report
- search
- subscribe
- trace
- unlock
- unsubscribe

虽然的很多方式，但常用的就那几个。

要路由转换为无效JavaScript变量名称的方法，请使用括号表示法。 例如，app ['m-search']（'/'，函数....

`app.all()`是作用于所有路由的。

### app.param([name], callback)

为路由添加一个路由参数的触发函数。参数中的name是parameter的name值或其数组。callback是回调方法。其参数依次是request, response, next middleware, 参数的value, 参数的name。

若参数中的name是一个数组。callback会在每次被声明时执行。执行顺序是定义顺序。除最后一个中间件外每一个中间件都需要执行`next()`方法进入下一个中间件。

下面是一个例子，当`:user`出现在路由路径中，你可以映射逻辑去提供`req.user`,或执行验证。

```
app.param('user', function (req, res, next, id) {
  // try to get the user details from the User model and attach it to the request object
  User.find(id, function (err, user) {
    if (err) {
      next(err)
    } else if (user) {
      req.user = user
      next()
    } else {
      next(new Error('failed to load user'))
    }
  })
})
```

参数回调函数是被声明在当地路由上，它不会继承app的路由。因此，只要app的路由中的有时监听的路由参数就会触发。

所有的参数回调函数都会先于路由回调函数触发，只有在请求回馈周期内执行一次，即使这个参数可以匹配多个路由。下面是一个例子。

```
app.param('id', function (req, res, next, id) {
  console.log('CALLED ONLY ONCE')
  next()
})

app.get('/user/:id', function (req, res, next) {
  console.log('although this matches')
  next()
})

app.get('/user/:id', function (req, res) {
  console.log('and this matches too')
  res.end()
})
// On GET /user/42, the following is printed:
// CALLED ONLY ONCE
// although this matches
// and this matches too
```

```js
app.param(['id', 'page'], function (req, res, next, value) {
  console.log('CALLED ONLY ONCE with', value)
  next()
})

app.get('/user/:id/:page', function (req, res, next) {
  console.log('although this matches')
  next()
})

app.get('/user/:id/:page', function (req, res) {
  console.log('and this matches too')
  res.end()
})
// On GET /user/42/3, the following is printed:
// CALLED ONLY ONCE with 42
// CALLED ONLY ONCE with 3
// although this matches
// and this matches too
```

> 下面的内容在express v4.11.0后才可以使用。

我不会。

### app.path()

返回一个相对于app的权威的路径。

```
var app = express()
var blog = express()
var blogAdmin = express()
app.use('/blog', blog)
blog.use('/admin', blogAdmin)
console.dir(app.path()) // ''
console.dir(blog.path()) // '/blog'
console.dir(blogAdmin.path()) // '/blog/admin'
```

有时这个方法会得到非常复杂的path（string）.有比它更好的方法。可以使用`req.baseUrl`.

### app.post(path, callback [, callback ...])

### app.put(path, callback [, callback ...])

### app.render(view, [locals], callback)

返回一个被回调函数渲染后的html。它接受一个options参数（就是上面的locals）。它包括视图需要的局部变量。就像`res.render()`除了不能发送给客户端。

> `app.render()`方法是一个非常有用的方法。其内部的`res.render()`方法使用了`app.render()`方法渲染视图。
>
> 局部变量`cache`是控制是否缓存视图的。若我想在开发时缓存视图需要设置为true,在生产环境下默认cache是true.

```
app.render('email', function (err, html) {
  // ...
})

app.render('email', { name: 'Tobi' }, function (err, html) {
  // ...
})
```

### app.route(path)

返回一个单独路由的实例，在其上可以使用http请求方式、中间件。使用`app.route()`可以避免路由名重复和拼写错误。

```
var app = express()

app.route('/events')
  .all(function (req, res, next) {
    // runs for all HTTP verbs first
    // think of it as route specific middleware!
  })
  .get(function (req, res, next) {
    res.json({})
  })
  .post(function (req, res, next) {
    // maybe add a new event...
  })
```

### app.set(name, value)

可以使用这个方法设置任意你需要使用的变量的值。

`app.set('foo', true)` <=> `app.enable('foo')`

`app.set('foo', false)` <=> `app.disable('foo')`

检索值是使用`app.get()`

```
app.set('title', 'my site')
app.get('title') // my site
```



### app.use([path,] callback [, callback ...])



**说明**

因为路由默认为`"/"`，所以应用程序的每个请求都会执行没有路由安装的中间件。下面的中间件会作用于每一个请求。

```
app.use(function (req, res, next) {
	console.log('time: %d', Date.now())
	next()
})
```

> 子应用不会继承有默认值设置。所以你必须在子应用中设置值。
>
> 子应用会继承没有默认值的设置。

因为中间件是依次执行的，所以其定义顺序很重要。

```
// 这个中间件不会被执行，因为它在定义路由前定义。
app.use(function (req, res, next) {
  res.send('hello world')
})
// 给路由绑定中间件
app.get('/', function (req, res) {
  res.send('welcome')
})
```

#### 错误控制中间件

错误中间件总是提供4个参数。（err, req, res, next）

```
app.use(function (err, req, res, next) {
  console.log(err.stack)
  res.status(500).send('Something broke')
})
```

现在是一些简单的例子。

##### 使用path

```
app.use('/abcd', (req, res, next) => {
next()
})
```

##### 使用path pattern

```
app.use('/abc?d', (req, res, next) => {
	next()
})
app.use('/ab+cd', (req, res, next) => {
	next()
})
app.use('/ab\*cd', (req, res, next) => {
	next()
})
app.use('/a(bc)?d', (req, res, next) => {
	next()
})
```

##### 使用regular expression

```
app.use(/\/abc|\/xyz/, (req, res, next) => {
	next()
})
```

##### 使用Array

```
app.use(['/abcd', '/xyza', /\/lmn|\/pqr/], (req, res, next) => {
	next()
})
```

#### 中间件回调函数的例子

下面是一些例子，以中间件作为app.use()/app.METHOD()/app.all()方法的回调函数。若然都是使用app.use()，对于app.use()/app.METHOD()/app.all()同样。

##### 单个中间件

```
//
app.use((req, res, next) => {
	next()
})
//
var router = express.Router()
router.get('/', (req, res, next) => {
	next()
})
//
var subApp = express()
subApp.get('/', (req, res, next) => {
	next()
})
```

##### 多个中间件

```
// 定义多个中间件在同一个路由上
var r0 = express.Router()
r0.get('/', (req, res, next) => {
	next()
})
r1 = express.Router()
r1.get('/', (req, res, next) => {
	next()
})
app.use(r1, r2)
```

##### 中间件组成的数组

```
// 使用中间件组成的数组。你必须明确使用路由才可以使用这样的数组。
var r0 = express.Router(),
	r1 = express.Router()
r0.get('/', (req, res, next) => {
	next()
})
r1.get('/', (req, res, next) => {
	next()
})
app.use('/', [r1, r2])
```

##### 多种形式组成的中间件序列

```
function mw0 (req, res, next) { next() }
function mw1 (req, res, next) { next() }
var r0 = express.Router(),
	r1 = express.Router(),
	subApp = express()
r0.get('/', (req, res, next) => { next() })
r1.get('/', (req, res, next) => { next() })
subApp.get('/', (req, res, next) => { next() })
app.use(mw0, [mw1, r0, r1], subApp)
```

下面是一个使用express.static中间件的例子。保存静态内容到‘public’目录。

```
// get style.css etc
app.use(express.static(__dirname + '/public'))
```

在/static路由下挂载一个提供静态资源的中间件。当请求前缀为/static的路由是执行该中间件。

```
app.use('/static', express.static(__dirname + '/public'))
```

通过在静态中间件之后加载记录器中间件来禁用静态内容请求的日志记录。

```
app.use(express.static(__dirname + '/public'))
app.use(logger())
```

使用多个静态资源时，后定义的会优先于先定义的。

```
app.use(express.static(__dirname + '/public'))
app.use(express.static(__dirname + '/files'))
app.use(express.static(__dirname + '/uploads'))
```



































### app.use()

#### app.use([path,] callback [, callback...])

在指定的路径上绑定指定的中间件。当路由被匹配时执行这个中间件。

##### Arguments

| 参数     | 说明 | 默认值 |
| -------- | ---- | ------ |
| path     |      |        |
| callback |      |        |



##### Description

##### Error-handling middleware

##### Path examples

##### Middleware callback function example
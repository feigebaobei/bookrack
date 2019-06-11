# Response

express应用在得到请求时回馈给请求的对象。一般使用res变量表示，也可以使用你喜欢的变量。下面是例子。

```
app.get('/user/:id', function (req, res) {
  res.send('user ' + req.params.id)
})
app.get('/user/:id', function (req, res) {
  res.send('user ' + req.params.id)
})
```

res对象在node的高版本时是内置字段和方法[build-in field and methods](<https://nodejs.org/api/http.html#http_class_http_serverresponse>)

## Propertise
### res.app

在中间件中使用express应用的实例。

### res.headerSent

是否使用http返回。

### res.locals

包含回馈时的数据的对象。存在于请求/返回周期。

```
app.use(function (req, res, next) {
  res.locals.user = req.user
  res.locals.authenticated = !req.user.anonymous
  next()
})
```

## Methods
### res.append(field [, value])

> 在express v4.11.0+可以使用

在http返回头部设置字段及值。

提示：若先使用res.set()再使用res.append()会重置返回头。

```
res.append('Link', ['<http://localhost/>', '<http://localhost:3000/>'])
res.append('Set-Cookie', 'foo=bar; Path=/; HttpOnly')
res.append('Warning', '199 Miscellaneous warning')
```

### res.attachment([filename])

设置返回头的Content-Disposition为'attachment'。若提供了filename，则设置Content-Type为其扩展名（使用res.type()）。并设置Content-Disposition"filename=" parameter.

```
res.attachment()
// Content-Disposition: attachment
res.attachment('path/to/logo.png')
// Content-Disposition: attachment; filename="logo.png"
// Content-Type: image/png
```

### res.cookie(name, value [, options])

为cookie设置字段及值。其值可以是string,也可以是object。

options有以下属性。

| 属性     | 类型           | 说明                                                         |
| -------- | -------------- | ------------------------------------------------------------ |
| domain   | String         | cookie的主域名，默认为app的主域名。                          |
| encode   | Function       | 用来异步编码的方法。默认为encodeURIComponent                 |
| expires  | Date           | 设置cookie的GMT过期时间。若不明确设置或设置为0，则创建一个会话。 |
| httpOnly | Boolean        | 标明cookie是否只接收web服务。                                |
| maxAge   | Number         | 设置期满时间与当前时间的毫秒值。                             |
| path     | String         | 设置cookie的路径，默认为'/'                                  |
| secure   | Boolean        | 标明cookie是否只使用https请求。                              |
| signed   | Boolean        | 指明cookie是否应该被标识。                                   |
| sameSite | Boolean/String | 其值是'Set-Cookie'设置的值。更多信息请查看[这里](<https://tools.ietf.org/html/draft-ietf-httpbis-cookie-same-site-00#section-4.1.1>) |

> res.cookie()设置cookie时使用options不非默认数据。

下面是一些例子：

```
res.cookie('name', 'tobi', { domain: '.example.com', path: '/admin', secure: true })
res.cookie('rememberme', '1', { expires: new Date(Date.now() + 900000), httpOnly: true })

// Default encoding
res.cookie('some_cross_domain_cookie', 'http://mysubdomain.example.com', { domain: 'example.com' })
// Result: 'some_cross_domain_cookie=http%3A%2F%2Fmysubdomain.example.com; Domain=example.com; Path=/'
// Custom encoding
res.cookie('some_cross_domain_cookie', 'http://mysubdomain.example.com', { domain: 'example.com', encode: String })
// Result: 'some_cross_domain_cookie=http://mysubdomain.example.com; Domain=example.com; Path=/;'

res.cookie('rememberme', '1', { maxAge: 900000, httpOnly: true })

res.cookie('cart', { items: [1, 2, 3] })
res.cookie('cart', { items: [1, 2, 3] }, { maxAge: 900000 })

res.cookie('name', 'tobi', { signed: true })
```

你可以使用[req.signedCookie](<http://expressjs.com/en/4x/api.html#req.signedCookies>)接收数据。

### res.clearCookie(name [, opitons])

清除cookie里的name值（使用默认值）。

```
res.cookie('name', 'tobi', { path: '/admin' })
res.clearCookie('name', { path: '/admin' })
```

### res.download(path [, filename] [, options] [, fn])

> 该方法的options参数在v4.16.0后开始支持。

使用attachment与转换后的文件路径。浏览器直接下载。默认使用filename为文件名。当有错误发生时触发fn方法。把options传递给底层的res.sendFile()方法并使用相同的参数。

```
res.download('/report-12345.pdf')
res.download('/report-12345.pdf', 'report.pdf')
res.download('/report-12345.pdf', 'report.pdf', function (err) {
  if (err) {
    // Handle error, but keep in mind the response may be partially-sent
    // so check res.headersSent
  } else {
    // decrement a download credit, etc.
  }
})
```

### res.end([data] [, encoding])

结束返回。这个方法来处node core.特别的[http.ServerResponse的response.end()方法](<https://nodejs.org/api/http.html#http_response_end_data_encoding_callback>)

若你不需要返回内容，则可以使用res.end()。否则请使用res.send()/res.json()等。

```
res.end()
res.status(404).end()
```

### res.format(object)

现在根据请求头的Accept字段判断内容。其内容列出Accept的值，若Accept没有明确给定数据，则执行第一个方法，若给定的值不在列表里，则返回406"Not Acceptable"或执行default。

虽然返回头里的Content-Type在回调函数里设置，但是你可以在res.set()/res.type()改变。

下面的例子里当Accept为"application/json"/"*/json"时会返回{"message": "hey"}。

```
res.format({
  'text/plain': function () {
    res.send('hey')
  },
  'text/html': function () {
    res.send('<p>hey</p>')
  },
  'application/json': function () {
    res.send({ message: 'hey' })
  },
  'default': function () {
    // log the request and respond with 406
    res.status(406).send('Not Acceptable')
  }
})
```

除了最经典的MIME类型外，你可以使用扩展名。如下：

```
res.format({
  text: function () {
    res.send('hey')
  },
  html: function () {
    res.send('<p>hey</p>')
  },
  json: function () {
    res.send({ message: 'hey' })
  }
})
```

### res.get(field)

返回http返回头的特定的字段。这个方法对大小定敏感。

```
res.get('Content-Type') // 'text/plain'
```

### res.json([body])

返回json格式的数据。参数可以是包括object/array/string/boolean/number等。

```
res.json(null)
res.json({user: 'tobi'})
res.status(500).json({error: 'message'})
```

### res.jsonp([body])

返回jsonp格式的数据。

```
res.jsonp(null)
// => callback(null)
res.jsonp({ user: 'tobi' })
// => callback({ "user": "tobi" })
res.status(500).jsonp({ error: 'message' })
// => callback({ "error": "message" })
```

jsonp的回调函数默认是callback。你可以使用[jsonp callback name](<http://expressjs.com/en/4x/api.html#app.settings.table>)可以推翻默认方法名。

下面是不使用默认方法名的例子：

```
// ?callback=foo
res.jsonp({ user: 'tobi' })
// => foo({ "user": "tobi" })
app.set('jsonp callback name', 'cb')
// ?cb=foo
res.status(500).jsonp({ error: 'message' })
// => foo({ "error": "message" })
```

### res.links(links)
### res.location(path)
### res.redirect([status,] path)
### res.render(view [, locals] [, callback])
### res.send([body])
### res.sendFile(path [, options] [, fn])

> 在v4.8.0后开始支持。

在给定的路径上传输文件。根据文件的扩展名设置Content-Type属性值。除非在options里设置了root，否则path必须使用绝对路径。

> 这个api提供数据给文件系统。就应该确保路径是安全的。需要root选项是绝对路径。
>
> 当root选项被提供。path是相对路径时，express会验证给定的root和path.

下面是options的属性

| 属性         | 说明                                                         | 默认值  | 支持版本 |
| ------------ | ------------------------------------------------------------ | ------- | -------- |
| maxAge       | 设置头信息里的Cache-Control里的max-age.单位为毫秒或毫秒形式的string | 0       |          |
| root         | 文件的相对根目录。                                           |         |          |
| lastModified | 设置头信息里的Last-Modified。若设置为false则不使用。         | Enabled | 4.9.0+   |
| headers      | 包含http头部                                                 |         |          |
| dotfiles     | 设置如何服务点文件。可选值：allow deny ignore                | ignore  |          |
| acceptRanges | 是否接受range请求                                            | true    | 4.14+    |
| cacheControl | 是否在头里设置Cache-Control。                                | true    | 4.14+    |
| immutable    | 是否立即直接返回Cache-Control。若为true，则需要maxAge指明缓存时间。它会保护客户端的文件执行有条件地请求，在maxAge内时请求会检测文件是否改变。 | false   | 4.16+    |

这个方法的回调方法（fn）会在传输完毕、出错时执行。当错后立即执行。

下面是一些例子：

```
app.get('/file/:name', function (req, res, next) {
  var options = {
    root: path.join(__dirname, 'public'),
    dotfiles: 'deny',
    headers: {
      'x-timestamp': Date.now(),
      'x-sent': true
    }
  }
  var fileName = req.params.name
  res.sendFile(fileName, options, function (err) {
    if (err) {
      next(err)
    } else {
      console.log('Sent:', fileName)
    }
  })
})
```

更多信息请查看[send](<https://github.com/pillarjs/send>)

### res.sendStatus(statusCode)
### res.set(field [, value])
### res.status(code)
### res.type(type)
### res.vary(field)


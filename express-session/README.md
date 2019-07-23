# Introduction

[TOC]

这是一个node.js模块，使用npm registry注册，使用下面命令安装。

`npm i express-session`

## API

session数据保存在server-side, cookie里保存session ID.  

### session(options)

#### options

##### cookie

default value `{path: '/', httpOnly: true, secure: false, maxAge: null}`

###### cookie.domain
###### cookie.expires
###### cookie.httpOnly
###### cookie.maxAge
###### cookie.path
###### cookie.sameSite
###### cookie.secure
##### genid

指定生成session id的方法。该方法若需要使用req参数，则req作为该方法的第一个参数。该方法默认使用[uid-safe](https://www.npmjs.com/package/uid-safe)库生成session id

##### name

cookie中session id （作为value）的name（作为key）。即`req.cookies.name // get session id`

default value is `connect.sid`

##### proxy

设置安全cookie时信任返回代理。

##### resave

是否重复保存session store. default true

##### rolling

是否在每次请求时重置maxAge为原始值。 default false

##### saveUninitialized

是否保存‘未初始化’的session.

当session是新的并未被修改时是未初始化。

##### secret
##### Required option
##### story

指定session仓库。默认是memorystore实例。

##### unset
#### req.session

req.session 可以设置、提取session 数据。下面是一个视图计数器。

```
// Use the session middleware
app.use(session({ secret: 'keyboard cat', cookie: { maxAge: 60000 }}))
// Access the session as req.session
app.get('/', function(req, res, next) {
  if (req.session.views) {
    req.session.views++
    res.setHeader('Content-Type', 'text/html')
    res.write('<p>views: ' + req.session.views + '</p>')
    res.write('<p>expires in: ' + (req.session.cookie.maxAge / 1000) + 's</p>')
    res.end()
  } else {
    req.session.views = 1
    res.end('welcome to the session demo. refresh!')
  }
})
```



#### Session.regenerate(callback)

调用该方法生成session。一旦被创建，新的SID、session实例会被初始化在res.session里。callback会被调用。

`req.session.regenerate(err => {})`

#### Session.destroy(callback)

删除这个session，不设置req.session属性。执行该方法时执行callback方法。

`req.session.regenerate(err => {})`

#### Session.reload(callback)

从store里重新加载这个session并放入req.session里。执行该方法时执行callback方法。

`req.session.reload(err => {})`

#### Session.save(callback)

`req.session.save(err => {/*session saved*/})`

#### Session.touch()

更新`maxAge`属性。

#### req.session.id()

req.sessionID()

##### req.session.cookie()

##### Cookie.maxAge
##### Cookie.originalMaxAge
#### req.sessionID
## Session Store Implementation

session 仓库工具。每一个session仓库都是事件驱动，并执行相应事件。下面是必要的、推荐的、可选的方法的列表。下面的方法在执行是都会执行callback.

### store.all(callback) 

可选择的（optional）

get all session from the store. callback(error, sessions)

### store.destroy(sid, callback)

必要的。（required）

destroy/delete session from the store given a session id(sid)。 

callback(error)

### store.clear(callback)

把store里的所有的session 都删除了。

callback(error)

### store.length(callback)

get the count of all session in the store.

callback(error, len)

### store.get(sid, callback)

根据sid得到session.

callback(null, null)

### store.set(sid, session, callback)

把story里的sid对应的session设置为指定的session.

callback(error)

### store.touch(sid, session, callback)

我不会。

## Compatible Session Stores

下面列出了很多session store(session仓库)

[connect-mongo](https://www.npmjs.com/package/connect-mongo)

[connect-redis](https://www.npmjs.com/package/connect-redis)

[session-file-store](https://www.npmjs.com/package/session-file-store)

## 例子

```
var express = require('express')
var parseurl = require('parseurl')
var session = require('express-session')
var app = express()
app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: true
}))
app.use(function (req, res, next) {
  if (!req.session.views) {
    req.session.views = {}
  }
  // get the url pathname
  var pathname = parseurl(req).pathname
  // count the views
  req.session.views[pathname] = (req.session.views[pathname] || 0) + 1
  next()
})
app.get('/foo', function (req, res, next) {
  res.send('you viewed this page ' + req.session.views['/foo'] + ' times')
})
app.get('/bar', function (req, res, next) {
  res.send('you viewed this page ' + req.session.views['/bar'] + ' times')
})
```


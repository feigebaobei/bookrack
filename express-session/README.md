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
##### rolling
##### saveUninitialized
##### secret
##### Required option
##### story
##### unset
#### req.session
#### Session.regenerate(callback)
#### Session.destroy(callback)
#### Session.reload(callback)
#### Session.save(callback)
#### Session.touch()
#### req.session.id()
#### req.session.cookie()
#### Cookie.maxAge
#### Cookie.originalMaxAge
#### req.sessionID
## Session Store Implementation
### store.all(callback)
### store.clear(callback)
### store.length(callback)
### store.get(sid, callback)
### store.set(sid, session, callback)
### store.touch(sid, session, callback)
## Compatible Session Stores
## 例子


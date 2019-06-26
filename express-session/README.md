# Introduction

[TOC]

这是一个node.js模块，使用npm registry注册，使用下面命令安装。

`npm i express-session`

## API

### session(options)

#### options

##### cookie
###### cookie.domain
###### cookie.expires
###### cookie.httpOnly
###### cookie.maxAge
###### cookie.path
###### cookie.sameSite
###### cookie.secure
##### genid
##### name
##### proxy
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
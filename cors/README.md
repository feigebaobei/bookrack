# Introduction

[TOC]

cors是node.js的资源共享的模块。可以支持Connect/Express框架。

## install

`npm i cors`

## usage

### simple usage(enable all cors requests)

```
var express = require('express')
var cors = require('cors')
var app = express()
app.use(cors())
app.get('/products/:id', (req, res, next) => {
    res.json({msg: 'string'})
})
```

### enable cors for a single route

```
app.get('/products/:id', cors(), (req, res, next) => {
	res.json({msg: 'string'})
})
```

### configuring cors

```
var corsOptions = {
    origin: 'http://example.com',
    optionsSuccessStatus: 200
}
app.get('/products/:id', cors(corsOptions), (req, res, next) => {
    res.json({msg: 'string'})
})
```

### configuring cors w/dynamic origin

```
var whitelist = ['http://example0.com', 'http://example1.com']
var corsOptions = {
    origin: (origin, callback) => {
        if (whitelist.indexOf(origin) !== -1) { // 这里只做了client请求的过滤。
            callback(null, true)
        } else {
            callback(new Error('string')
        }
    },
    optionsSuccessStatus: 200
}
app.get('/products/:id', cors(corsOptions), (req, res, next) => {
    res.json({msg: 'string'})
})

// 若希望不阻塞服务器请求服务器的请求。则如下：
var whitelist = ['http://example0.com', 'http://example1.com']
var corsOptions = {
    origin: (origin, callback) => {
        if (whitelist.indexOf(origin) !== -1 || !origin) { // 只有这里不一样。
            callback(null, true)
        } else {
            callback(new Error('string')
        }
    },
    optionsSuccessStatus: 200
}
app.get('/products/:id', cors(corsOptions), (req, res, next) => {
    res.json({msg: 'string'})
})

```

### enabling cors Pre-Flight

```
// 只预检一个路由
app.options('/products/:id', cors())
app.delete('/products/:id', cors(), (req, res, next) => {
    res.json({msg: 'string'})
})
// 预检所有路由
app.options('*', cors())
```

### configuring cors asynchronously

```
// 配置异步cors
var whitelist = ['http://example0.com', 'http://example1.com']
var corsOptionsDelegate = (req, cb) => {
    var corsOptions = {}
    if (whilelist.indexOf(req.header('Origin')) !== -1) {
        corsOptions = {origin: true}
    } else {
        corsOptions = {origin: false}
    }
    cb(null, corsOptions)
}
app.get('/products/:id', cors(corsOptionsDelegate), (req, res, next) => {
    res.json({msg: 'string'})
})
```

## configuration options

| options              | type           | description                                                  | default |
| -------------------- | -------------- | ------------------------------------------------------------ | ------- |
| origin               | Boolean        | true - 正常回应请求。false - 不可共享资源。                  |         |
|                      | String         | 指定的origin。只有该origin可以共享。                         |         |
|                      | RegExp         | 满足该正则的origin可以共享资源。                             |         |
|                      | Array          | 可以共享资源的origin组成的数组。                             |         |
|                      | Function       | 自定义的逻辑。第一个参数是：请求的origin。第二个请求是：cb.  |         |
| methods              | String / Array | 配置Access-Control-Allow-Methods。如`'GET'`,`'GET','POST','DELETE'`,`['POST', 'PUT']` |         |
| allowedHeaders       | String / Array | 配置Access-Control-Allow-Headers。                           |         |
| exposeHeaders        | String / Array | 配置Access-Control-Expose-Headers。如：`['Content-Range', 'X-Content-Range']` |         |
| credentials          | Boolean        | 配置Access-Control-Allow-Credentials。true: 正常回应。false: 省略。 |         |
| maxAge               | Int            | 配置Access-Control-Max-Age.若是整数，则正常回应，否则省略。  |         |
| preflightContinue    |                | 把cors预检请求传递给下一个处理程序。                         |         |
| optionsSuccessStatus |                | 提供OPTIONS请求的状态码。                                    |         |

options的默认值是

```
{
    "origin": "*",
    "methods": "GET,HEAD,PUT,PATCH,POST,DELETE",
    "preflightContinue": false,
    "optionsSuccessStatus": 204,
}
```
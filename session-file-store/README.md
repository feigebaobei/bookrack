# Introduction

[TOC]

用在express/connect/koa的session文件存储的中间件。默认保存在`./session`目录。

## 兼容性

express 4.x

connect  1.4.0

node.js 4

koa 0.9.0

## 开始

```
npm i session-file-store // 
npm install
npm test
```

## 控制项

- `path` The directory where the session files will be stored. Defaults to `./sessions`
- `ttl` Session time to live in seconds. Defaults to `3600`
- `retries` The number of retries to get session data from a session file. Defaults to `5`
- `factor` The exponential factor to use for retry. Defaults to `1`
- `minTimeout` The number of milliseconds before starting the first retry. Defaults to `50`
- `maxTimeout` The maximum number of milliseconds between two retries. Defaults to `100`
- `reapIntervalObject` [OUT] Contains intervalObject if reap was scheduled
- `reapInterval` Interval to clear expired sessions in seconds or -1 if do not need. Defaults to `1 hour`
- `reapAsync` use distinct worker process for removing stale sessions. Defaults to `false`
- `reapSyncFallback` reap stale sessions synchronously if can not do it asynchronously. Default to `false`
- `logFn` log messages. Defaults to `console.log`
- `fallbackSessionFn` returns fallback session object after all failed retries. No defaults
- `secret` if secret string is specified then enables encryption of the session before writing the file and also decryption when reading it.
- `encryptEncoding` Encryption output encoding. Defaults to `'hex'`
- `encoding` Object-to-text text encoding. Can be null. Defaults to `'utf8'`
- `encoder` Encoding function. Takes object, returns encoded data. Defaults to `JSON.stringify`
- `decoder` Decoding function. Takes encoded data, returns object. Defaults to `JSON.parse`
- `fileExtension` File extension of saved files. Defaults to `'.json'`
- `keyFunction` Encryption key retrieval function. Takes secret and session id, returns key. Defaults to `function(secret, sessionId){return secret + sessionId;}`

## 用法

由于express 4.0的改变。需要我们把express-session传递给session-file-store，以便扩展session.Store。

```
var session = require('express-session')
var FileStore = require('session-file-store')(session)
var fileStoreOptions = {}
app.use(session({
	store: new FielStore(fileStoreOptions),
	secret: 'keyboard cat'
}))
```

## 例子

这里有一个[基本例子](<https://github.com/valery-barysok/session-file-store/tree/master/examples>)。

```
var express = require('express');
var app = express();
var session = require('express-session');
var FileStore = require('session-file-store')(session);

app.use(session({
    store: new FileStore,
    secret: 'keyboard cat',
    resave: true,
    saveUninitialized: true
  })
);

app.get('/', function (req, res) {
  if (req.session.views) {
    req.session.views++;
    res.setHeader('Content-Type', 'text/html');
    res.write('<p>views: ' + req.session.views + '</p>');
    res.end();
  } else {
    req.session.views = 1;
    res.end('Welcome to the file session demo. Refresh page!');
  }
});

var server = app.listen(1337, function () {
  var host = server.address().address;
  var port = server.address().port;

  console.log('Example app listening at http://%s:%s', host, port);
});
© 2019 GitHub, Inc.
```


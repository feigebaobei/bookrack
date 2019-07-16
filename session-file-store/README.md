# Introduction

在Express/Connect/Koa框架下可以保存session file。

session-file-store就是session数据保存在session file里。

### 兼容

express >= 4.x connect >= 1.4.0

node.js >= 6

koa >= 0.9.0

### 安装

`npm i session-file-store`

### demo

```
var session = require('express-session')
var FileStore = require('session-file-store')(session)
var fileStoreOptions = {}
app.use(session({
	store: new FileStore(fileStoreOptions),
	secret: 'keyboard cat'
}))
```

### options

session-file-store需要一个对象作为参数（也可省略）。下面是这个对象的各个属性的说明。

| options            | 说明                                                         | 默认值                                                     |
| ------------------ | ------------------------------------------------------------ | ---------------------------------------------------------- |
| path               | 指定session file保存在什么地方。                             | ./sessions                                                 |
| ttl                | session存活多少秒                                            | 3600                                                       |
| retries            | 尝试多少次从session file里得到session数据                    | 5                                                          |
| factor             | retries 的指数                                               | 1                                                          |
| minTimeout         | 多少ms后执行第一次取session数据                              | 50                                                         |
| maxTimeout         | 最多多少ms后再次尝试取session数据                            | 100                                                        |
| reapIntervalObject |                                                              |                                                            |
| reapInterval       |                                                              |                                                            |
| reapAsync          |                                                              |                                                            |
| reapSyncFallback   |                                                              |                                                            |
| logFn              | 日志消息                                                     | console.log                                                |
| fallbackSessionFn  | 当取session数据全部失败时触发                                | -                                                          |
| secret             | 若指定该值，则在写入session前加密session.读取session时解密。 | -                                                          |
| encryptEncoding    | 加密输出编码                                                 | hex                                                        |
| encoding           | 把对象变为文本。可以是null.                                  | utf-8                                                      |
| encoder            | 编码的方法。该方法的参数是一个对象返回编码后的文本。         | JSON.stringify                                             |
| decoder            | 解码方法。                                                   | JSON.parse                                                 |
| fileExtension      | 文件扩展名。                                                 | .json                                                      |
| keyFunction        | 加密字段的方法。该方法的参数secret / session id，返回加密后的key. | `function (secret, sessionId) {return secret + sessionId}` |


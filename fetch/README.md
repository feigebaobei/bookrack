# Introduction

从指定的url中提取内容。支持zip和快速下载。

## install

`npm i fetch`

## usage

```
fetch.fetchUrl(url[, options], cb)
fetch.fetchStream(url[, options], cb)
```

## fetch from url

`fetch.fetchUrl(url[, options], cb)`

url:

options: 

cb: 回调函数 callback(error, meta, body)

## streaming

`fetch.FetchStream(url[, options]) -> Stream`

url: 

options: 

### 事件

- data 
- meta 关于返回的信息
- end 当准备开始接收数据时
- error

```
var FetchStream = require("fetch").FetchStream;
var fetch = new FetchStream("http://google.com");
fetch.on("data", function(chunk){
    console.log(chunk);
});
```

## opitons

| options            | describe                                                     | default  | demo |
| ------------------ | ------------------------------------------------------------ | -------- | ---- |
| maxRedirects       | 最大可以重定向的数量。                                       |          |      |
| disableRedirects   | 是否不允许重写向                                             | false    |      |
| headers            | header字段。使用键值对的格式书写。                           |          |      |
| maxResponseLength  | 最大的文件长度。若超出则截去超出的部分。                     | Infinity |      |
| method             | 请求方式                                                     | GET      |      |
| payload            | request body                                                 |          |      |
| disableGzip        | 是否可以包含gzipping.                                        |          |      |
| cookies            | 定义cookies。使用数组的形式`['name=value']`                  |          |      |
| cookieJar          | 各请求中共享cookie。                                         |          |      |
| outputEncoding     | 对fetchUrl验证。                                             |          |      |
| disableDecoding    | 对于fetchUrl验证，若为true，则自动设置为`utf-8`              |          |      |
| overrideCharset    | 对于 fetchUrl，设置input encoding.                           |          |      |
| asyncDnsLookup     | 使用基于c-ares的高性能异步dns解析。不使用getaddrinfo(3)的连接池。 |          |      |
| timeout            | 设置超时的ms.                                                |          |      |
| agentHttps         | 对于https的agent参数。                                       |          |      |
| agentHttp          |                                                              |          |      |
| rejectUnauthorized | 是否拒绝自签名的证书。                                       |          |      |

### meta object

| options         | describe | default | demo |
| --------------- | -------- | ------- | ---- |
| status          |          |         |      |
| responseHeaders |          |         |      |
| finalUrl        |          |         |      |
| redirectCount   |          |         |      |
| cookieJar       |          |         |      |

## piping to file

```
var FetchStream = require('fetch').FetchStream,
	fs = require('fs'),
	out
out = fs.createWriteStream('file.html')
new FetchStream('http://www.baidu.com').pipe(out)
```


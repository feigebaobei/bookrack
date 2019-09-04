# Introduction

[TOC]

## motivation

把要window.fetch带入到node.js里的轻量级模块。

## features

- 使用window.fetch api
-  
- 可以使用原生的promise，也可以使用promise库。
- 在request/response之间使用原生的node流作为body。
- 使用gzip/deflate编码、解码，并自动转化成string输出。
- 还有很多扩展项。如timeout/redirect/response size等。

## difference from client-side fetch

## install

`npm i node-fetch`

## loading and configuring the module

```
const fetch = require('node-fetch')
// config
const Bluebird = require('bluebird')
fetch.Promise = Bluebird
```

## common usage

### plain text or html

```
fetch('https://github.com/').then(res => res.text()).then(body => console.log(body))
```

### json

```
fetch('url').then(res => res.json()).then(json => console.log(json))
```

### simple post

```
fetch('url', {method: 'POST', body: 'a=1'}).then(res => res.json()).then(json => console.log(json))
// 注意：body里是string
```

### post with json

```
const body = {a: 1}
fetch('url', {
	method: 'post', // 大小写都行
	data: JSON.stringify(body),
	headers: {'Content-Type': 'application/json'}
}).then(res => res.json()).then(json => console.log(json))
```

### post with form parameters

```
const {URLSearchParams} = require('url')
const params = new URLSearchParams()
params.append('a', 1)
fetch('url', {
	method: 'post',
	body: params
}).then(res => res.json()).then(json => console.log(json))
```

### handing exceptions

```
fetch('url').catch(err => console.log(err))
```

### handling client and server errors

```
function checkStatus(res) {
	if (res.ok) {// res.status >= 200 && res.status < 300
		return res
	} else {
		throw myCustomError(res.statusText)
	}
}
fetch('url').then(checkStatus).then(res => console.log('will not get here...'))
```

## advanced usage

### streams

### buffer

### accessing headers and other meta data

### extract Set-Cookie header

### post data using a file stream

### post with form-data(detect multipart)

### request cancellation with AbortSignal

## api

### fetch(url[, options])

### options

#### default headers

| header | value |
| ------ | ----- |
|        |       |
|        |       |
|        |       |
|        |       |
|        |       |
|        |       |
|        |       |

#### custom agent

### class: Request

#### new Request(input[, options])

### class: Response

#### new Response([body[, options]])

#### response.ok

#### response.redirected

### class: Headers

#### new Headers([init])

### interface: Body

#### body.body

#### body.bodyUsed

#### body.arrayBuffer()

#### body.blob()

#### body.json()

#### body.text()

#### body.buffer()

#### body.textConverted()

### class: FetchError

### class: AbortError
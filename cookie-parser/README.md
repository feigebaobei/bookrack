# Introduction

它可以把req.cookie里有数据按照cookie的name合成到req.cookie里。你可以使得secret设置签名cookie。也可以

## 安装

`$ npm i cookie-parser`

## API

```
var express = require('express')
var cookieParser = require('cookie-parser')
var app = express()
app.use(cookieParser())
```

### cookieParser(secret, options)

`secret`是string/array用于签名cookies。若没有明确指定这项，则不解析签名cookies。若使用string，则用作密码。若使用Array，则试图依次使用array的项解密每一个cookie。

`options`传递给cookie.parser的第二个参数。有关更多信息请查看cookie.

`decode`是为cookie的值解码的方法。

### cookieParser.JSONCookie(str)

把cookie解析为json。若传入的值是json cookie，则返回json。否则返回传入的值。

### cookieParser.JSONCookie(cookie)

传入一个对象，将可以迭代对象的每一个属性执行JSONCookie，把执行结果代替原来的值。返回与传入相同的值。

### cookieParser.signedCookie(str, secret)

把cookie值解析为签名cookie。若是签名cookie或签名有效，则返回不签名cookie。若不是签名cookie，则返回原则。若是签名cookie，但是无效，则返回false。

`secret`参数可以是一个数组。若是string，则当做secret使用。若是数组，则试图依次每一个secret不签名cookie.

### cookieParser.signedCookie(cookie, secret)

传入一个对象，迭代每一个属性并检测其值是否签名。若已签名并且有效，则使用返回的对象代替这个对象。

secret可以是string/array。若是string，则当作secret使用。若是array，则试图依次不签名每一个cookie.

## 例子

```
var express = requrie('express')
var cookieParser = require('cookie-parser')
var app = express()
app.use(cookieParser())
app.get('/', (req, res) => {
	console.log('Cookies', req.cookies)
	console.log('Signed Cookies: ', req.signedCookies)
	// 设置
	// res.cookie('k', 'v', {options})
	// 得到
	// req.cookies
})
app.listen(8080)
```


# Introduction

解析body的中间件。

解析请求体的中间件。解析后使用`req.body`得到请求体。

提示：在使用请求体的内容前请先验证。

# installation

`npm i body-parser`

# api

引入body-parser。

`var bodyParser = require('body-parser')`

bodyParser对象会暴露出几个可以生成中间件的方法。每中间件解析请求体是需要bodyParser的解析方法（即type）与请求头的`Content-Type`的值相同。若不同则报错。若相同且请求体不为空则返回该解析方法的处理后的值。若相同且请求体为空则返回空对象。

使用方法：`bodyParser.<TYPE>([options])`

content-type的类型如下：

- application/json
- application/x-www-form-urlencoded
- text/plain
- application/octet-stream
- multipart/form-data

## bodyParser.json([options])

返回的中间件只能解析json数据，并且`Content-Type: application/json`

| option  | 说明                                                         | 数据类型                          | default            |
| ------- | ------------------------------------------------------------ | --------------------------------- | ------------------ |
| inflate | 是否解压                                                     | boolean                           | true               |
| limit   | 指定最大的请求体                                             | string/number                     | '100kb'            |
| reviver | 指定JSON.parse的第二个参数                                   | boolean                           |                    |
| strict  | 是否接收任意类型。若为true则只接受array/object。若为false则接受任意类型。 | boolean                           | true               |
| type    | 确定该中间件需要解析的媒体类型。                             | string/array of string / fn(req). | 'application/json' |
| veryfy  | 指定验证方法。若指定，则执行`verify(req, res, buf, encoding)`去验证。它可以抛出错误中止解析。 |                                   |                    |



## bodyParser.raw([options])
## bodyParser.text([options])
## bodyParser.urlencoded([options])

返回一个只能解析`urlencoded`的中间件。并且请求头里的`Content-Type`必须为`application/x-www-form-urlencoded`.解析时只接收`UTF-8`格式编码。支持自动解压gzip 、 deflate。

把解析后的数据放在req.body里。若extended为false，则值为string/array。若extended为true，则值为任意格式。

| opitons        | 说明                                                         | 数据类型                           | defautl                             |
| -------------- | ------------------------------------------------------------ | ---------------------------------- | ----------------------------------- |
| extended       | 选择哪个库去解析。若设为true，则使用qs库。若设为false则使用querystring库。 | boolean                            | true                                |
| inflate        | 若为true，则解压。若为false，则不解压。                      | boolean                            | true                                |
| limit          | 控制请求体的最大值。                                         | string/number                      | '100kb'                             |
| parameterLimit | 控制最大请求参数数量。超过最大值时返回413状态码。            | number                             | 1000                                |
| type           | 设置media type。                                             | sting / array of string / function | 'application-x-www-form-urlencoded' |
| verify         | 指定验证方法。若指定，则执行`verify(req, res, buf, encoding)`去验证。它可以抛出错误中止解析。 |                                    |                                     |

## bodyParser.json([options])

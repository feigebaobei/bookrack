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
| veryfy  | 指出验证。若指定则执行`verify(req, res, buf, encoding)`。它可以抛出错误中止解析。 |                                   |                    |



## bodyParser.raw([options])
## bodyParser.text([options])
## bodyParser.urlencoded([options])

## bodyParser.json([options])

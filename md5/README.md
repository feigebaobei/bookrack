# Introduction

用于hasing信息。

## install

`npm i md5`

## api

`md5(message)`

message: String / Buffer

return String

## usage

```
var md5 = require('md5')
console.log(md5('string'))
var fs = require('fs')
fs.readFile('first.txt', (err, buf) => {console.log(md5(buf))})
```


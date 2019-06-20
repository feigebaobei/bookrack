# Introduction

使用一些安全方法解析、字符串化查询字符串。

## 使用

```
var qs = require('qs');
var assert = require('assert');
 
var obj = qs.parse('a=c');
assert.deepEqual(obj, { a: 'c' });
 
var str = qs.stringify(obj);
assert.equal(str, 'a=c');
```

### 解析对象

### 解析数组

### 字符串化

### 控制null值

### 处理特殊字符集


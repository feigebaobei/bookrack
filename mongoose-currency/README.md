# Introduction

## 这是什么

- 将字符串通常保存为数字。（通过剥离非数字并乘以100）以保证精度。
- 删除开头的字符串。
- 删除逗号，
- 只保存2位小数。且不舍入。
- 剥离去[a-zA-Z].
- 传入一个字符串、数字。数字会被原样保存。
- 你若设置值为整数，则认为你已完成转换。若传递一个浮点数，将四舍五入它。只需传入整数即可。

## 怎么使用

```
var mongoose = require('mongoose');
var Schema = mongoose.Schema;
// Will add the Currency type to the Mongoose Schema types
require('mongoose-currency').loadType(mongoose);
var Currency = mongoose.Types.Currency;
 
// If you don't have the Currency variable declared you can use 'mongoose.Types.Currency'
var ProductSchema = Schema({
  price: { type: Currency }
});
 
var Product = mongoose.model('Product', ProductSchema);
 
var product = new Product({ price: "$1,200.55" });
product.price; // Number: 120055
product.price = 1200;
product.price; // Number 1200 It will not round or multiply. Stored AS IS and should represent $12.00
```



## 测试

## 去做

## 得到精度


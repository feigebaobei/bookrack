# Schema

[TOC]

## defining your  schema

所有的事件都是从schema开始的，每一个schema都反映了mongodb的集合，也定义了每个文档（集合包含文档）的结构。

```
var mongoose = require('mongoose')
var Schema = mongoose.Schema
var blogSchema = new Schema({
	title: String,
	date: {
		type: Date,
		default: Date.now
	}
})
```

上面的代码是定义schema的示例。若定义后还相添加key，则使用`schema.add(cb)`方法。每个key的path(轨道)都有SchemaType。下面是SchemaType的可选项。[Schema的详细说明](./schema.md)。

- String
- Number
- Date
- Buffer
- Boolean
- Mixed
- ObjectId
- Array
- Decimal128 // 128位的小数
- Map

## creating a model

`var Blog = mongoose.model('Blog', blogSchema)`

## instance methods

schema有好多内置的方法。schema也可以添加自定义的方法。

```
blogSchema.methods.fn = function(cb) {
	// ..
}
blogSchema.method(fn, function (cb) {
	// ...
})
```

- 反对修改schema内置方法。
- Schema.methods.fn = fn 与 Schema.method(fn, fn) 的作用一样。
- 若fn使用es6 箭头函数（=>）则不使用this了。

## statics

```
blogSchema.statics.fn = function(name) {
	return this.find({name: new RegExp(name, 'i')})
}
blogSchema.static('fn', function(bread) {
	return this.find({bread})
})
```

- schema.statics 与 Schema#static() 的作用一样。
- 使用箭头函数需要注意this。

静态方法可以作用于当前schema。

实例方法中对当前实例有作用。

## query helpers

查询助手可以方便查询。

```
blogSchema.query.byName = function (name) {
	return this.where({name: new RexExp(name, 'i')})
}
```



blogSchema

## indexes

## virtuals

## aliaes

## options

### autoIndex

### autoCreate

### bufferCommands

### capped

### collection

### id

### _id

### 还有好多

## pluggable
# Schema

[TOC]

## defining your  schema

所有的事件都是从schema开始的，每一个schema都反映了mongodb的集合，也定义了每个文档（集合包含文档）的结构。

```
var mongoose = require('mongoose')
let {Schema} = mongoose
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

Schemas不只定义文档结构，也可以定义文档实例的方法，静态model的方法，复合键。在文档生命周期内都可以使用这些中间件。

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

查询助手可以方便查询。让你可以使用链式查询。

```
blogSchema.query.byName = function (name) {
	return this.where({name: new RexExp(name, 'i')})
}
var Animal = mongoose.model('Animal', animalSchema)
Animal.find().byName('fido').exec((err, animals) => {
	console.log(animals)
})
```

## indexes

monodb支持二等索引。在schema里为每一个定义了index的字段自动执行`createIndex`方法。Mongoose会依次执行creatIndex,当全部成功或出错时执行`index`事件。因`creatIndex`会造成重大性能问题，所以推荐在生产环境不要使用。使用`autoIndex`设置是否使用index.

```
// 下面这4种写法都可以mongoose.connect('mongodb://localhost:27017/database', {autoIndex: false})
mongoose.createConnect('mongodb://localhost:27017/database', {autoIndex: false})
mongoose.set('autoIndex', false)
new Schema({...}, {autoIndex: false})
```

更多信息可以查看Model#ensureIndexes方法。

## virtuals

虚拟字段就是不保存在mongodb里的。使用`set`/`get`方法设置、得到其值。示例如下：

```
var personSchema = new Schema({
	name: {
		first: String,
		last: String
	}
})
personSchema.virtual('fullName').get(() => name.first + name.last).set(v => {
    name.first = v.substr(0, v.indexOf(' '))
    name.last = v.substr(v.indexOf(' '))
})
var Person = mongoose.model('Person', personSchema)
var axl = new Person({
    name: {
    	first: 'Axl',
    	last: 'Rose'
    }
})
// get
axl.fullName // 'Axl Rose'
// set
axl.fullName = 'Willam Rose' // first: 'Willam' last: 'Rose'
```

因虚拟属性不保存在数据库中，所以不能使用虚拟属性查询。

## aliaes

为字段设置别名。

```
var childSchema = new Schema({
	name: {
		type: String,
		alias: 'n'
	}
})
var Child = mongoose.model('Child', childSchema)
var ming = new Child({
	name: 'shism'
})
// get
ming.name // 'shism'
```

## options

shemas有好多配置项。

```
new Schema({...}, options)
// or
var schema = new Schema({...})
schema.set(option.value)
```

合法的选项有：

- autoIndx
- ……

| option             | type            | describe                                                     | default | demo                                                         |
| ------------------ | --------------- | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| autoIndex          | Boolean         | 是否使用索引（index）                                        | true    |                                                              |
| capped             | Number / Object | 设置集合的最大字节。                                         |         | ` {capped: 1024}`,`{capped: {size: 1024, max: 1000, autoIndexId: true}}` |
| autoCreate         | Boolean         | 是否使用校对选项建立集合。若已经存在集合，且该集合的校对项有与设置的校对项不同时会抛出错误。所以在生产环境通常设置为false. 在创建索引之前会执行`Model.createCollection()` |         |                                                              |
| bufferCommands     | Boolean         | 是否使用缓冲命令。schema的bufferCommands选项会覆盖全局的bufferCommands选项。 |         |                                                              |
| collection         | String          | 指定model对应的集合的名称。集合名称默认使用model的名称。     |         |                                                              |
| id                 | Boolean         | 是否使用返回文档`_id`字段的`id`getter.                       |         |                                                              |
| _id                | Boolean         | 在Schema中是否使用与`ObjectId`一致的`_id`字段。              |         |                                                              |
| minimize           | Boolean         | 是否保存一个空对象。若要为false则不保存。取时会返回undefined |         |                                                              |
| read               | String          | 查询时使用偏好。                                             |         |                                                              |
| writeConcern       | Object          | 我不会。[mongoDB 写安全](<https://blog.csdn.net/weixin_33713503/article/details/90285557>) |         |                                                              |
| shardKey           |                 | 我不会                                                       |         |                                                              |
| strict             | Boolean         | 是否可以不指定schema时保存数据。                             |         |                                                              |
| strictQuery        |                 |                                                              |         |                                                              |
| toObject           |                 | 把文档里的数据转化为js object                                |         | `{getters: true}`                                            |
| toJSON             |                 |                                                              |         | `{getters: true, virtuals: false}`                           |
| typeKey            |                 |                                                              |         |                                                              |
| validateBeforeSave | Boolean         | 是否在保存前验证。                                           |         |                                                              |
| versionKey         |                 |                                                              |         |                                                              |
|                    |                 |                                                              |         |                                                              |
|                    |                 |                                                              |         |                                                              |
|                    |                 |                                                              |         |                                                              |
|                    |                 |                                                              |         |                                                              |
|                    |                 |                                                              |         |                                                              |
|                    |                 |                                                              |         |                                                              |
|                    |                 |                                                              |         |                                                              |
|                    |                 |                                                              |         |                                                              |



### autoIndex

是

### autoCreate

### bufferCommands

### capped

### collection

### id

### _id

### 还有好多

## pluggable
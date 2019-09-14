# Populate

populate可以为当前文档添加其它档。可以使用此功能实现与关系型数据库相似的功能。

```
const mongoose = require('mongoose')
const Schema = mongoose.Schema
const personSchema = Schema({
	_id: Schema.Types.ObjectId,
	name: String,
	age: Number,
	stories: [
		{
			type: Schema.Types.ObjectId,
			ref: 'Story' // 这个model会用于填充数据。
		}
	]
})
const storySchema = Schema({
	author: {
		type: Schema.Types.ObjectId,
		ref: 'Person'
	},
	title: String
	fans: [
		{
			type: Schema.Types.ObjectId,
			ref: 'Person'
		}
	]
})
const Story = mongoose.model('Story', storySchema)
const Person = mongoose.model('Person', personSchema)
```

提示：`ObjectId`,`Number`,`String`,`Buffer`都可以作为合法的ref值。若你不是熟练的程序员，请使用`ObjectId`.

## saving refs

把refs的值保存到其它文文档需要分配_id。

```
const author = new Person({
	_id: new mongoose.Types.ObjectId(),
	name: 'name',
	age: 38
})
author.save(err => {
	if (err) {
		return handleError(err)
	}
	const story1 = new Story({
		title: 'title',
		author: author._id // 一定要指定_id
	})
	story1.save(err => {
		if (err) return handlerError(err)
	})
})
```

## Populating

查询出填充的内容。

```
Story.fineOne({title: 'title'})
	.populate('author') // 指定填充的字段
	.exec((err, story) => {
		if (err) {
			return handleError(err)
		}
		console.log('The author is %s', story.author.name)
	})
```

填充的路径的值不再是_id，而是从数据库查询到的文档。

## Setting Populated Fields

```
Story.findOne({title: 'title'}, function (error, story) {
	if (error) {
		return handleError(error)
	}
	story.author = author
})
```

## What If There'a No Foreign Document?

mongoose没有连接查询。当story.author没有文档时，会返回null.populate类似左连接查询

```
const story = await Story.findOne({title: 'asdf'}).populate('author')
story.author // null
```

```
const storySchema = Schema({
	authors: [
		{
			type: Schema.Types.ObjectId,
			ref: 'Person'
		}
	],
	title: String
})
const story = await Story.findOne({title: 'title'}).populate('authors')
story.authors // []
```

## Field Selection

只返回填充文件的指定字段。

```
Story.findOne({title: 'title'}).populate('author', 'name') // 只返回name字段。
.exec((err, story) => {
	if (err) {
		return handleError(err)
	}
	console.log(story.author.name) // name
	console.log(story.author.age) // null
})
```

## Populating Multiple Paths

一次查询多个填充路径。

```
Story.find(...).populate('fans').populate('author').exec()
```

若多次查询同一个填充路径，则最后一个启作用。

## Query conditions and other options

```
Story.find(...).populate({
	path: 'fans',
	match: {age: {$gte: 21}},
	select: 'name -_id',
	options: {limit: 5}
}).exec()
```

## Refs to children

```
Story.find({author: author._id})
	.exec((err, stories) => {
		if (err) {
			return handleError(err)
		}
		...
	})
```

## Populating an existing document

使用document#populate()为已经存在的文档添加填充路径。

```
demo
```

## Populating multiple existing documents

Model.populate()为多个已经存在的文档填充路径。

Model.populate(docs, options[, cb(err, doc)]) 

docs: 文档 、文档组成的数组。

options: 

| options | type    | describe                     | default | demo |
| ------- | ------- | ---------------------------- | ------- | ---- |
| path    | string  | 填充方法的路径。             |         |      |
| select  | string  | 查询的字段。                 |         |      |
| match   |         | 匹配的查询条件。             |         |      |
| model   |         | 用于填充的model的name.       |         |      |
| options | object  | 查询的条件。如：sort / limit |         |      |
| justOne | boolean | 是否设置为数组。             |         |      |

返回填充文档。

```
modelName.populate(doc, {path: 'name', function(err, users) {...}})
```

## Populating across multiple levels

```
// 得到朋友的朋友
User.findOne({name: 'value'})
	.populate({
		path: 'friends',
		populate: {
			path: 'friends'
		}
	})
```

## Populating across Databases

```
modelInstance.find().populate({path: 'conversation', model: otherModelInstance}).exec((err, docs) => {...})
```

## Dynamic Reference via `refPath`

mongoose可以填充来自文档的属性的值的多个集合。一般用于可以支持多个schema的schema。

```
// no.1 方法
const commentSchema = new Schema({
	body: {
		type: String,
		required: true,
		refPath: 'onModel' // 根据body的值分别使用不同的schema填充。
	},
	onMondel: {
		type: String,
		required: true,
		enum: ['BlogPost', 'Product']
	}
})
const Product = mongoose.model('Product', new Schema({name: String}))
const BlogPost = mongoose.model('BlogPost', new Schema({title: String}))
const Comment = mongoose.model('Comment', commentSchema)

// no.2 方法
constt commentSchema = new Schema({
	body: {...},
	product: {
		type: Schema.Types.ObjectId,
		required: true,
		ref: 'Product'
	},
	blogPost: {
		type: Schema.Types.ObjectId,
		required: true,
		ref: 'BlogPost'
	}
})
```

## Populate Virtuals

Schema.prototype.virtual(name, options)

name: 虚拟字段的key.

| option       | type              | describe                   | default |
| ------------ | ----------------- | -------------------------- | ------- |
| ref          | string / model    | model的名字或model的实例。 |         |
| localField   | string / function | 在本文档里的字段名。       |         |
| foreignField | string / funciton | 在外链方档的字段名。       |         |
| justOne      | Boolean           | 是否以数组的形式返回值。   | false   |
| count        | Boolean           | 是否只包含文档的数量。     | false   |

```
Band.find(...).populate({path: 'members', select: 'name'}).exec(...)
```

## Populate Virtuals: The Count Option

// 得到数量

```
doc.virtual('numMembers', {
	ref: 'Person',
	localField: 'name',
	foreignField: 'bank',
	count: true
})
doc.findOne({name: 'name'}).populate('numMembers')
doc.numMembers // 2
```

## Populate in Middleware

可以在pre / post 钩子函数上填充文档。

```
MySchema.pre('find', function () {
	populate('user')
})
```


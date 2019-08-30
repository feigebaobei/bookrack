# Models

models使用已经定义过的schema做为文档的结构。文档是model的一个实例。model负责向mongodb数据库创建、读取文档。

## 编译model

```
var schema = new mongoose.Schema({name: String, size: String})
var Tank = mongoose.model('Tank', schema)
```

在mongodb中会以model的名称的小写的复数做为集合的名字。这个例子中的集合名称就是`tanks`

## constructing documents

文档是model的一个实例。很容易创建、保存数据。

```
var Tank = mongoose.model('Tank', schema)
var small = new Tank({size: 'small'})
small.save(err => {
	if (err) {
		return handleError(err)
	}
})
// or
Tank.create({size: 'small'}, (err, small) => {
	if (err) {
		return handleError(err)
	}
})
// or, 一次保存一批数据
Tank.insertMany([{size: 'small'}], (err) => {
	if (err) {
		return handleError(err)
	}
})
```

每一个model使用一个数据库连接。当使用`mongoose.model()`时使默认的mongoose连接。若需要使用指定连接，需要创建。

```
var connection = mongoose.createConnection('mongodb://localhost:27017/test')
var Tank = connection.model('Tank', yourSchema)
```

## quering

`Tank.find({size: 'small'}).where('createDate').gt(oneYearAgo).exec(callback)`

更多查询请看query api.

## deleting

```
Tank.deleteOne({size: 'large'}, (err) => {
	if (err) return handlerError(err)
})
```

## updating

```
Tank.updateOne({size: 'large'}, {name: 'T-90'}, (err, res) => {...})
```

## change streams

chanege stream会监听插入、更新的方法。但是若没有连接mongodb的副本集，则change stream不工作。


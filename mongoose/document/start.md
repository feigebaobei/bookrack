# Quick Start

### 安装

`npm i mongoose`

### 连接数据库

```
var mongoose = require('mongoose')
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParse: true})
```

### 连接对象的事件

连接数据时有可能成功有可能失败，对应的方法分别是error/open.

```
var db = mongoose.connection
db.on('error', () => {
	console.log('error string')
})
db.once('open', () => {...})
```

### 创建model（就是为model设置字段）

```
var kittySchema = new mongoose.Schema({name: String})
var Kitten = mongoose.model('Kitten', kittySchema)
```

### 使用model

```
var silence = new Kitten({name: 'Silence'})
console.log(silence)
```

### 为schema添加方法

```
kittySchema.methods.speak = () => {
	console.log(name ? name : 'no name')
}
var Kitten = mongoose.model('Kitten', kittySchema)
```

### 使用model的方法

```
var fluffy = new Kitten({name: 'fluffy'})
fluffy.speak()
```

### 保存数据

save方法的参数是一个回调方法。其第一个参数是error.当出错是为真。

```
fluffy.save((err, fluffy) => {
	if (err) {
		return console.log(err)
	}
	fluffy.speak()
})
```

### 查询数据

多model里查询数据。

```
// 查询全部数据
Kitten.find((err, kittens) => {
	if (err) {
		return console.log(err)
	}
	console.log(kittens)
})
// 筛选部分数据
Kitten.find({name: /^fluff/}, callback) // 查找以fluff开发的name的数据。callback是回调方法。
```


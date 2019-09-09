# Connections

你可以使用`mongoose.connection()`连接mongodb。

`mongoose.connect('mongodb://localhost:27017/myapp', {useNewUrlParser: true})`

也可以连接特定的mongodb

`mongoose.connect('mongodb://username:password@host:port/database?options...', {useNewUrlParser: true})`

这是有2个配置项，第一个是连接字符串的配置项。第二个是mongodb底层驱动的配置项。

## operation buffering

mongoose可以让你立即使用mongodb。不用等待建立连接。mongoose有了缓存机制后方便了使用，也是常见混淆问题的源头。而且mongoose在没有连接时不会报出error。你可以在`schema`里使用`bufferCommands`控制是否使用缓存。也可以在全局设设置

`mongoose.set('bufferCommands', false)`

## error  handling

有2种error.

1. 建立连接时出错（或失败）。`mongoose.connection()`会触发promise对象的reject。
2. 建立连接后出错，mongoose会触发error事件。

使用下面的方法控制这2种error.

```
// 处理连接失败
mongoose.conncet(url, {useNewUrlParser: true}).catch(error => handlerError(error))
// or
try {
	await mongoose.connect(url, {useNewUrlParser: true})
} catch (err) {
	handlerError(err)
}
// 处理error事件
mongoose.connect(url, {useNewUrlParser: true})
```


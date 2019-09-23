# Middleware

中间件是控制异步执行期间传递控制的方法。

## Types of Middleware

一共分为4类中间件。

1. document middleware（支持的方法：validate / save / remove / init）
2. query middleware (支持的方法：count / deleteMany / deleteOne / find / findOne / findOneAndDelete / findOneAndRemove / findOneAndUpdate / remove/ update / updateOne / updateMany)
3. aggregate middleware (支持的方法：aggregate)
4. model middleware (支持的方法：insertMany)

所有的中间件都支持pre / post hook.

注意：如果指定schema.pre（'remove'），Mongoose默认会为doc.remove（）注册此中间件。 如果您希望中间件在Query.remove（）上运行，请使用schema.pre（'remove'，{query：true，document：false}，fn）。

注意：create（）函数触发save（）钩子。

## Pre

pre 中间件会在前一个执行完后再执行下一个。所以每个中间件都会使用`next()`调用下一个中间件。

在mongoose 5.x中可以使用返回promise对象代替next()。在实际开发中再结合async/await更方便。

```
schema.pre('save', async funciton () {
	await doStuff()
	await doMoreStuff()
})
```

在使用next()时不会中止执行其后面的代码。若希望中止需要使用return。

### 用处

1. 用于原子性的模块逻辑。
2. 复杂的验证。
3. 去除依赖文档。
4. 默认异步。
5. 异步任务。

### Errors in Pre Hooks

当pro hook 出现error时mongoose会中止执行其后代码。可以抛出error或返回promise对象。

```
schema.pre('save', funciton (next) {
	const error = new Error('string')
	next(error)
})
schema.pre('save', function () {
	return new Promise((res, rej) => {
		rej(new Error('str'))
	})
})
schema.pre('save', function() {
  // You can also throw a synchronous error
  throw new Error('something went wrong');
});

schema.pre('save', async function() {
  await Promise.resolve();
  // You can also throw an error in an `async` function
  throw new Error('something went wrong');
});

// later...

// Changes will not be persisted to MongoDB because a pre hook errored out
myDoc.save(function(err) {
  console.log(err.message); // something went wrong
});
```

## Post

post中间件会在钩子函数后执行，并且所有的pre中间件执行完后执行。

```
schema.post('init', function(doc) {
  console.log('%s has been initialized from the db', doc._id);
});
schema.post('validate', function(doc) {
  console.log('%s has been validated (but not saved yet)', doc._id);
});
schema.post('save', function(doc) {
  console.log('%s has been saved', doc._id);
});
schema.post('remove', function(doc) {
  console.log('%s has been removed', doc._id);
});
```

## Asynchronous Post Hooks

当post钩子函数有至少2中间件时，mongoose会认为第二个参数是next。`next()`会触发下一个中间件。

## Define Middleware Before Compiling Models

在编译模型前绑定插件才会起作用。

```
const schema = new mongoose.Schema({name: String})
schema.pre('save' () => console.log('string'))
const User = mongoose.model('User', schema)
new User({name: 'test'}).save()
```

若把mongoose model放入单独的一个文件处理并返回定义好的schema。则需要在使用require之前使用全局定义插件（mongoose.plugin(fn)）

## Save/Validate Hooks

`save()`会触发`validate()`.即所有的`pre('validate')`和`post('validate')`执行完后执行`pre('save')`和`post('save')`

## Naming Conflicts

区分`Document.remove()``Model.remove()`

```
schema.pre('remove', {document: true}, () => {console.log('removing doc')})
schema.pre('remove', {query: true}, () => {console.log('removing query')})
```



## Notes on findAndUpdate() and Query Middleware
## Error Handling Middleware
## Aggregation Hooks
## Synchronous Hooks
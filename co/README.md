# Introduction

使用promises为nodejs和浏览器提供基于生成器的控制流优度，让你以一种很好的方式编写非阻塞代码。

## co v4

co的n4版返回的是promise对象。

```
co(function* () {
  var result = yield Promise.resolve(true);
  return result;
}).then(function (value) {
  console.log(value);
}, function (err) {
  console.error(err.stack);
});
```

使用`co.wrap(fn*)`返回一个返回promise对象的方法。

```
var fn = co.wrap(function* (val) {
  return yield Promise.resolve(val);
});
 
fn(true).then(function (val) {
 
});
```

## platform compatibility



## install

`npm i co`

## associated libraries



## examples

```
let co = require('co')
let onerror = (err) => {
	console.error(err.stack)
}
co(function * () {
	var result = yield Promise.resolve(true)
}).catch(onerror)
co(function * () {
  var a = Promise.resolve(1);
  var b = Promise.resolve(2);
  var c = Promise.resolve(3);
  var res = yield [a, b, c];
  console.log(res);
}).catch(onerror)
co(function * () {
	try {
		yield Promise.reject(new Error('boom'))
	} catch( err ) {
		console.error(err.message)
	}
}).catch(onerror)
```

## yieldables

### Promises

### thunks

### arrays

yield会平行解决item

```
co(function*() {
	var res = yield [Promise.resolve(1),Promise.resolve(2),Promise.resolve(3)]
	console.log(res)
}).catch(onerror)
```

### objects

也是平行解决item

```
co(function * () {
	var res = yield {
		1： Promise.resolve(1),
		2： Promise.resolve(2)
	}
	console.log(res)
}).catch(onerror)
```

### generators and generator functions

## api

### co(fn*).then(val => )

通过解决generator对象或generator方法或返回generator方法的方法，返回一个promise对象。然后就可以使用then、catch。

### var fn = co.wrap(fn*)

把generator方法转化为promise对象。


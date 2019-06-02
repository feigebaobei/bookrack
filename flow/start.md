# getting started

# install

flow是一个静态检测js代码的工具。它可心提高你有工作效率。让你写代码更快，更小，更自信，让你做更大的事儿。

flow若检测到错误会以静态注释的方法标注出来。如：在写代码时标明参数数据类型，flow就会确保代码使用正确数据类型安全运行。

```js
function square(n: number): number {
	return n * n
}
```

因为flow可以理解js代码，所以我们可以不明确写出数据类型。我们只需要最小化地（开心地，当然也不最小化。）编写我们的代码。flow会推断静态代码里的问题。在大多时候flow可以很好地理解我们的代码里的类型。

```javascript
// @flow
function square(n) {
	return n * n // Error!
}
square('2')
```

[现在我们就去学习如何使用它。](./install.md)


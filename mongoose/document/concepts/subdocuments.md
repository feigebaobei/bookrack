# Subdocuments

子文档有2个概念：子文档组成的数组、单个嵌套的子文档。

```
var parent = new Schema({
	child: childSchema,
	children: [childSchema]
})
```

## what is subdocument

当使用嵌套的文档，并使用验证、保存时。有先后顺序。

1. 执行父级文档验证。
2. 执行子级文档验证。
3. 执行子级文档保存。
4. 执行父级文档保存。

## finding a subdocument

因为每一个子文档都有一个`_id`字段。所以我们可以使用`id（）`找到相应的子文档。

`var doc = parent.childen.id(_id)`

## adding subdocs to Arrays

添加到子文档的数组里

```
// 从父文档里操作
parent.children.push({name: 'value'})
// 从子文档里操作
parent.children.create({name: 'value'})
```



## removing Subdocs

```
// 删除子文档组成的数组
parent.children.id(_id).remove() // 相当于 parent.children.id(_id).pull()
// 删除单个子文档
parent.child.remove()
```

## parents of Subdocs

```
docInstance.singleNested.parent() // parent docInstance
docInstance.singArr[0].parent() // parent docInstance
docInstance.level1.level2.level3.ownerDocument() // docInstance 返回顶级文档实例
```

### alternate declaration syntax for arrays

当使用Array / Object作为字段值的类型时。mongoose会自动使用Schema覆盖Array / Object.

```
var parentSchema = new Schema({
	children: [{name: 'string'}]
})
// Equivalent
var parentSchema = new Schema({
	children: [new Schema({name: 'string'})]
})
```


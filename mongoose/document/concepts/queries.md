# Queries

mongoose提供了一些静态方法在CRUD操作时会有返回一个`query`对象。

可以为query对象使用callback方法。也可以使用`.then()`

常用的crud操作。

| operation                 | describe                           | demo                                                 |      |
| ------------------------- | ---------------------------------- | ---------------------------------------------------- | ---- |
| Model.deleteMany()        | 删除全部满足条件的文档             | m.deleteMany({name: /Stark/}, function (err) {...})  |      |
| Model.deleteOne()         | 删除在集体中第一个满足条件的文档。 | `m.deleteOne({name: 'value'}, function (err) {...})` |      |
| Model.find()              | 返回全部满足筛选条件的文档。       |                                                      |      |
| Model.findById()          |                                    |                                                      |      |
| Model.findByIdAndDelete() |                                    |                                                      |      |
| Model.findByIdAndRemove   |                                    |                                                      |      |
| Model.findByIdAndUpdate() |                                    |                                                      |      |
| Model.findOne()           |                                    |                                                      |      |
| Model.findOneAndDelete()  |                                    |                                                      |      |
| Model.findOneAndRemove()  |                                    |                                                      |      |
| Model.findOneAndUpdate()  |                                    |                                                      |      |
| Model.replaceOne()        |                                    |                                                      |      |
| Model.updateMany()        |                                    |                                                      |      |
| Model.updataOne()         |                                    |                                                      |      |

## executing



## queries are not pormise



## reference to other documents

## streaming


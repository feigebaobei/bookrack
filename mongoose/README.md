# Introduction

为node.js设计的高雅的mongodb对象。

让我们面对现实吧，编辑mongodb验证，转换和业务逻辑样板是一种拖累。这就是我们写mongoose的原因。

```
const mongoose = require('mongoose')
mongoose.connect('mongodb://localhost:27017/test', {useNewUrlParser: true})
const Cat = mongoose.model('Cat', {name: String})
const kitty = new Cat({name: 'Zildjian'})
kitty.save().then(() => console.log('meow'))
```

mongoose直接提供了概要解决模板。包括内置的类型转换，验证，查询创建、业务逻辑、钩子函数等。


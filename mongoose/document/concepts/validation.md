# Validation

在学习验证之前需要了解以下规则。

- 验证被定义在SchemaType上。
- 验证是中间件。默认把验证绑定在每一个schema的pre('save')上。即保存前验证。
- 可以手动触发验证：`doc.validate(cb)`/`doc.validateSync()`
- 不可以验证没有定义的值。除非定义了required。
- 验证是异步递归的。
- 验证可以自定义。

## Built-in Validators

- 所有的SchemeTypes都内置了required验证。使用：`mongoose.Schema.Types.String.checkRequired(v => typeof v === 'string')`  
- Numbers有min / max验证方法。
- String有enum / match / minlength / maxlenght验证方法。

每一个验证器都会提供好多信息。包括如何通过验证，错误提示信息。

    var breakfastSchema = new Schema({
        eggs: {
            type: Number,
            min: [6, 'too few eggs'], // [需要满足的条件, '提示信息']
            max: 12
        },
        bacon: {
            type: Number,
            required: [true, 'why no bacon?']
        },
        drink: {
            type: String,
            enum: ['Coffee', 'Tea'],
            required: function () {
                return this.bacon > 3
            }
        }
    })

## The `unique` Option is Not a Validator

## Custom Validators

    function validator (val) {
        return val === 'something'
    }
    new Schema({
        name: { 
            type: String,
            validate: validator // 添加验证方法
        },
        age: {
            type: Number,
            validate: [ // 添加验证方法和对应的提示信息
                validator,
                'tip string'
            ]
        },
        grate: {
            type: Number,
            validate: [ // 一次添加多个验证器
                {
                    validator: validator,
                    msg: 'tip string'
                },
                {
                    validator: validator,
                    msg: 'tip string'
                }
            ]
        }
    })

### Error message templates

    schema.path('name').validate({
        validator: validator,
        message: function (props) { // 使用模板显示提示信息
            return `${props.path} must have length 5, got '${props.value}'`
        }
    })
    schema.path('name').validate({
        validator: function () {throw new Error('options!')}, // 使用错误里的信息显示提示信息
        message: function (props) {
            return props.reason.message
        }
    })

## Asynchronous validation

    var userSchema = new Schema({
        name: { // 使用promise时的demo
            type: String,
            validate: () => new Promise((resolve, reject) => {
                if (name instanceof String) {
                    resolve()
                } else {
                    reject()
                }
            })
        },
        email {
            type: String,
            validate: { // 使用回调时需要指明isAsync,验证器的第一个参数是value/第二个参数是回调函数.
                isAsync: true,
                validator: (v, cb) => {
                    setTimeout(function () {
                        cb(cb(v), msg)
                    }, 5)
                }
            },
            message: 'string'
        },
        required: [true, 'string']
    })

## Validation Error

当验证失败时会返回errors对象.它包括每一个字段的完整的错误信息(`ValidatorError`对象).validatorError对象包括kind/path/value/message属性。当验证方法返回error对象时，validatorError对象会多一个reason属性。其reason就是一个error对象。

    var toySchema = new Schema({
        color: String,
        name: String
        })
    var validator = (v) => {...}
    toySchema.path('color').validate(validator, 'string')
    toySchema.path('name').validate((v) => {...}, 'string')
    var Toy = db.model('Toy', toySchema)
    var toy = new Toy({color: 'Green', name: 'Power'})
    toy.save(err => {
        err.errors.color.message
        err.errors.color.kind
        err.errors.color.path
        err.errors.color.value
        err.errors.color.reason.message
    })

## Cast Errors

在验证前会尝试强制把值转换为指定类型的值。若转换失败则会返回包含`CastError`的errors对象，并且不会执行验证方法。验证方法的参数会被假定为null/undefined/指定的SchemaType.

    doc.save(err => {
        err.errors[field].name // 'CastError'
        err.errors[field].message // 'string'
    })

## Required Validators On Nested Objects

    var e = doc.validateSync()
    e.errors[field] // 

## Update Validators

默认更新验证不会开启。即：执行`updateOne() / updateMany() / findOneAndUpdate()`不会触发验证。当需要开启更新验证时需要指定`runValidators`为true.默认关闭的原因是它有几个中止程序的错误。

    var toySchema = new Schema({
      color: String,
      name: String
    });
    var Toy = db.model('Toys', toySchema);
    Toy.schema.path('color').validate(function (value) {
      return /red|green|blue/i.test(value);
    }, 'Invalid color');
    var opts = { runValidators: true };
    Toy.updateOne({}, { color: 'not a color' }, opts, function (err) {
      assert.equal(err.errors.color.message,
        'Invalid color');
    });

## Update Validators and this

文档的验证器里的this指向document. //
更新验证器的this指向undefined //

## The context option

context选项可以把this指向当下的query对象。

    var opts = { runValidators: true, context: 'query' };
    Toy.updateOne({}, update, opts, function(error) {
      assert.ok(error.errors['color']);
    });

## Update Validators Only Run On Updated Paths

```
doc.updateOne({}, {$unset: {name: 1}}, {runValidators: true}, (err) => {
	// ...
	// err.errors[field]
})
```

## Update Validators Only Run For Some Operations

更新验证只会运行在下列的更新操作上。

- $set
- $unset 
- $push(>=4.8.0)
- $addToSet
- $pull
- $pullAll

## On $push and $addToSet

4.8.0时新增了2个验证器：`$push`、`$addToSet`
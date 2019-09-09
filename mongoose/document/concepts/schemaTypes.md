# SchemaTypes

## what is a SchemaType?

为指定路径上的值去验证类型。

```
const schema = new Schema({name: String})
schema.path('name') instanceof mongoose.SchemaType // true
schema.path('name') instanceof mongoose.Schema.Types.String // true
schema.path('name').instance // 'String'
```

下面是SchemaTypes的列表

- String
- Number
- Date
- Buffer
- Boolean
- Mixed
- ObjectId
- Array
- Decimal128
- Map

下面是使用方法

```
var schema = new Schema({
    name: String,
    updated: {
        type: Date,
        default: new Date()
    },
    arr: [],
    ofDates: [Date],
    ofArrayOfNumbers: [[Number]],
    nested: {
        stuff: {
            type: String,
            lowercase: true,
            trim: true
        }
    },
    mapOfString: {
        type: Map,
        of: String
    }
})
```

### SchemaType Options // 作用于SchemaType 的配置项

| options | type | describe | default | demo |
| ------- | ---- | -------- | ------- | ---- |
|         |      |          |         |      |
- required Boolean / Function 
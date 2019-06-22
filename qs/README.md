# Introduction

















## 解析对象

`qs.parse(string, [options])`

qs允许你使用查询字符串来创建一个嵌套的对象。子关键字使用[]包裹。下面是一个例子。

```
qs.parse('foo[bar]=baz')
// 
{
    foo: {
        bar: 'baz'
    }
}
```

| 属性              | 类型          | 默认值 | 说明                 |
| ----------------- | ------------- | ------ | -------------------- |
| plainObjects      | Boolean       |        |                      |
| allowPrototypes   | Boolean       | false  | 是否允许重写属性。   |
| depth             | Number        | 5      | 解析深度。           |
| parameterLimit    | Number        | 1000   | 每层解析的属性数量。 |
| ignoreQueryPrefix | Boolean       | false  | 是否忽略请求前缀。   |
| delimiter         | String/RegExp | ''     | 使用什么分隔kv结构。 |
| allowDots         | Boolean       | false  | 是否允许点设置属性。 |
| charset           | String        |        |                      |
| charsetSentinel   | Boolean       |        |                      |
|                   |               |        |                      |
|                   |               |        |                      |

## 解析数组

qs也可以解析数组。使用[]符号。

```
qs.parse('a[]=b&a[]=c') // {a: ['b', 'c']}
```

可以明确设置下标

```
qs.parse('a[1]=b&a[0]=c') // {a: ['c', 'b']}
```

| 属性        | 类型    | 默认值 | 说明                                                         |
| ----------- | ------- | ------ | ------------------------------------------------------------ |
| arrayLimit  | Number  | 20     | 允许解析的数据的长度。若有下标超出该设置、设置小于0的值、使用非number设置，则会解析为对象。 |
| parseArrays | Boolean | true   | 是否解析为数组。                                             |
| comma       | Boolean | false  | 是否允许使用逗号分隔数组数据。                               |
|             |         |        |                                                              |
|             |         |        |                                                              |
|             |         |        |                                                              |
|             |         |        |                                                              |
|             |         |        |                                                              |
|             |         |        |                                                              |
|             |         |        |                                                              |
|             |         |        |                                                              |

下面是一个例子：

```
// 
qs.parse('a=b,c', {comma: true}) // {a: ['b', 'c']}
// 
qs.parse('a[][b]=c') // {a: [{b: 'c'}]}
// 
qs.parse('a[0]=b&a[b]=c') // {a: { '0': 'b', b: 'c' }}
// 
qs.parse('a[]=b', {parseArrays: false}) // {a: {'0': b}}
//
qs.parse('a[1]=b', {arrayLimit: 0}) // {a: {'1': 'b'}}
//
qs.parse('a[100]=b', {a: {'100': 'b'}})
// qs会把解析后的数据紧凑地返回。
qs.parse('a[2]=b&a[15]=c&a[0]=a') // {a: ['a', 'b', 'c']}
```

## 字符串化

`qs.stringify(object, [opitons])`

默认输出encode后的字符串。

| 属性             | 类型     | 默认值    | 说明                                                         |
| ---------------- | -------- | --------- | ------------------------------------------------------------ |
| encode           | Boolean  | true      | 是否输出encode后的字符串。                                   |
| encodeValuesOnly | Boolean  | false     |                                                              |
| encoder          | Function | null      | 使用自定义的编码器。fn的参数是每一个数据。                   |
| decoder          | Function | null      | 使用自定义的解码器。fn的参数是每一个数据。                   |
| arrayFormat      | String   | 'indices' | 使用指定格式返回字符串。indices:指明下标。brackets: 使用[]式。 repeat: 多次表达式。 comma: 使用逗号分隔。 |
| allowDots        | Boolean  | false     | 是否返回点形式。                                             |
| addQueryPrefix   | Boolean  | true      | 是否添加查询前缀。                                           |
| delimiter        | Boolean  | '&'       | 设置分界符。                                                 |
| serializaDate    | Function | null      | 此方法只对Date对象有用。设置序列化方法。                     |
| sort             | Function | null      | 使用指定的fn排序。该方法接受2个参数，可以使用这2个参数做比较。 |
| filter           |          |           |                                                              |

下面是一些例子：

```
qs.stringify({a: ['b', 'c', 'd']})
// 'a[0]=b&a[1]=c&a[2]=d'
qs.stringify({a: ['b', 'c', 'd']}, {indices: false})
// 'a=b&a=c&a=d'
qs.stringify({a: []})
// '' 空数组转换为字符串时返回空字符串。
qs.stringify({a: null, b: undefined})
// 'a=' 遇到undefined则忽略。

```

## 处理null

默认处理null与处理''一样。`qs.stringify({a: null, b: ''}) // a=&b=`

| 属性               | 类型    | 默认值 | 说明                                                         |
| ------------------ | ------- | ------ | ------------------------------------------------------------ |
| strictNullHandling | Boolean | false  | 是否严格处理null。这个属性作用于parse、stringify。作用于parse时，若key对应的value为null时，则返回的key没有value。当作用于stringify时，若key后面没有value(key后面是分隔符)时，则解析为null. |
|                    |         |        |                                                              |
|                    |         |        |                                                              |
|                    |         |        |                                                              |
|                    |         |        |                                                              |
|                    |         |        |                                                              |
|                    |         |        |                                                              |
|                    |         |        |                                                              |
|                    |         |        |                                                              |
|                    |         |        |                                                              |
|                    |         |        |                                                              |

## 处理特殊的字符集

字符集默认是utf-8。可以使用charset设置为iso-8859-1.

你也可以使用其它字符集。如：[Shift JIS](<https://en.wikipedia.org/wiki/Shift_JIS>) [qs-iconv](<https://github.com/martinheidegger/qs-iconv>).

```
// encode
var encoder = require('qs-iconv/encoder')('shift_jis')
var shiftJISEncoded = qs.stringify({ a: 'こんにちは！' }, { encoder: encoder })
// 'a=%82%B1%82%F1%82%C9%82%BF%82%CD%81I'
// decode
var decoder = require('qs-iconv/decoder')('shift_jis')
var obj = qs.parse('a=%82%B1%82%F1%82%C9%82%BF%82%CD%81I', { decoder: decoder })
// 'こんにちは！'
```

## RFC3986和RFC1738空格编码

rfc3986默认把`''`编码为`%20`.

rfc1738默认把`''`编码为`+`.


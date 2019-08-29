# Connections

## options

| option            | type          | describe                                                     | default      | demo |
| ----------------- | ------------- | ------------------------------------------------------------ | ------------ | ---- |
| bufferCommands    | Boolean       | 是否使用缓存机制                                             |              |      |
| user/pass         |               | 指定mongodb的username password。它们等同于mongodb驱动的auth.user / auth.password |              |      |
| autoIndex         | Boolean       | 是否使用自动生成index。                                      |              |      |
| dbName            | String        | 指定连接mongodb的database。它可以推翻连接字符串。            |              |      |
| useNewUrlParser   | Boolean       | 是否使用新的url解析。若为true，则url必须有port               | fauls        |      |
| useCreateIndex    | Boolean       | true:使用createIndex()建立，false:使用ensureIndex()建立。    | false        |      |
| useFindAndModify  | Boolean       | true:在使用`findOneAndUpdate``findOneAndRemove`时使用`findAndModify()`,否则使用`findOneAndUpdate()` | true         |      |
| reconnectTries    |               | 若链接的是单个服务或mongos proxy,则mongodb会每隔`reconnectInterval`ms执行`reconnectTries`次。然后放弃连接并触发`reconnectFailed`事件。 |              |      |
| reconnectInterval |               |                                                              |              |      |
| promiseLibrary    |               | promise库                                                    |              |      |
| poolSize          | Number        | 连接池的数量。                                               | 5            |      |
| bufferMaxEntries  | Number        | 在没有连接mongodb前缓存操作的数量                            | -1（不限制） |      |
| connectTimeoutMS  |               | 在连接后多长时间无操作时杀死soket（断开连接）。              | 30000        |      |
| socketTimeoutMS   |               | 在连接后多长时间无操作时杀死soket（断开连接）。              | 30000        |      |
| family            | Number(4 / 6) | 使用IPv4 / IPv6连接。mongodb会先用IPv6去连接，若失败，则再使用IPv4。若你想快点连接，可以使用`mongoose.connect(url, {family: 4})` |              |      |

## callback

mongoose.connect() 会返回promise对象。所以可以使用then/catch方法

## 连接字符串的配置项

这些配置项会作用于mongodb driver.

## connection 事件

| event          | describe                                                     |      |      |
| -------------- | ------------------------------------------------------------ | ---- | ---- |
| connecting     | 初始连接时触发                                               |      |      |
| connected      | 连接成功时                                                   |      |      |
| open           | 连接成功时                                                   |      |      |
| disconnecting  | 连接失败时                                                   |      |      |
| disconnected   | 连接失败后                                                   |      |      |
| close          | 成功断开连接时                                               |      |      |
| reconnected    | 再连接并成功时                                               |      |      |
| error          | 在连接成功后，出现错误时                                     |      |      |
| fullsetup      | 当您连接到副本集并且Mongoose已成功连接到主节点和至少一个辅助节点时发出。 |      |      |
| all            |                                                              |      |      |
| reconectFailed | 当您连接到独立服务器并且Mongoose已用完时发出                 |      |      |
| reconnectTries | 在最后一次尝试连接mongodb后触发。若连接复制集，则永远不会被触发。 |      |      |

## keepAlive

当长时间运行应用时，应该谨慎使用`keepAlive`。它可以避免出现`connection closed`的问题。
`mongoose.connect(uri, {keepAlive: true, keepAliveInitialDelay: 300000})`
keepAliveInitialDelay (ms) 连接成功保证socket活多长时间。

## replica set connections

直译成中文是复制集合连接，再看代码，更像连接多个host.
`mongoose.connect('mongodb://user:pw@host1.com:27017,host2.com:27017,host3.com:27017/test/db')`
连接单个node复制集合时，需要指明`replicatSet`配置项。
`mongoose.connect('mongodb://host1:port/?replicaSet=rsName')`

## replica set host names

## 支持Multi-mongos

`mongoose.connect('mongodb://mongosA:27501,mongosB:27501', cb)`

## Multiple connections

当需要连接多个database、或设置不使用权限（read/write）时。需要使用`mongoose.createConnection()`
`mongoose.createConnection('mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/database][?options]', options)`

## connection pools
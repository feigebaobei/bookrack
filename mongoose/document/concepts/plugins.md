# Plugins

schema是可以使用插件的。允许调用预先打包工具来扩展这些功能。

## example

若想要在数据库中添加最后修改时间字段。则使用插件会很简单。只需要在每一个schema上创建一个插件。

```
// lastMod.js
module.exports = exports = function lastModifiedPlugin (schema, options) {
  schema.add({ lastMod: Date });
  schema.pre('save', function (next) {
    this.lastMod = new Date();
    next();
  });
  if (options && options.index) {
    schema.path('lastMod').index(options.index);
  }
}

// game-schema.js
var lastMod = require('./lastMod');
var Game = new Schema({ ... });
Game.plugin(lastMod, { index: true });

// player-schema.js
var lastMod = require('./lastMod');
var Player = new Schema({ ... });
Player.plugin(lastMod);
```

结果是给gameschema添加lastMod轨迹，并设置为index。结playerSchema添加lastMod轨迹。

## global plugins

为model设置plugin后，该model的schema都会使用该plugin.

```
var mongoose = require('mongoose');
mongoose.plugin(require('./lastMod'));

var gameSchema = new Schema({ ... });
var playerSchema = new Schema({ ... });
// `lastModifiedPlugin` gets attached to both schemas
var Game = mongoose.model('Game', gameSchema);
var Player = mongoose.model('Player', playerSchema);
```



## officially supported plugins

- [mongoose-autopopulate](https://plugins.mongoosejs.io/plugins/autopopulate)
- [mongoose-lean-virtuals](https://plugins.mongoosejs.io/plugins/lean-virtuals)


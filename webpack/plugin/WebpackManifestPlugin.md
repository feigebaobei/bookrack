# WebpackManifestPlugin

用于生成资产清单的Webpack插件。在输入目录中生成一个资产清单。

## install

```
npm i -D webpack-manifest-plugin
```

## usage

```
var ManifestPlugin = require('webpack-manifest-plugin')
moduls.exports = {
	...
	plugin: [
		new ManifestPlugin
	]
}
```

## api(配置项)

| name            | type | default | describe |
| --------------- | ---- | ------- | -------- |
| filename        |      |         |          |
| publicPath      |      |         |          |
| basePath        |      |         |          |
| writeToFileEmit |      |         |          |
| seed            |      |         |          |
| filter          |      |         |          |
| map             |      |         |          |
| sort            |      |         |          |
| generate        |      |         |          |
| serialize       |      |         |          |

### FileDescriptor

```
FileDescriptor: {
	path: string;
  name: string | null;
  isInitial: boolean;
  isChunk: boolean;
  chunk?: Chunk;
  isAsset: boolean;
  isModuleAsset: boolean;
}
```


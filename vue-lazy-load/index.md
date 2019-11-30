# index

在移动端进行图片懒加载。

## usage

```
// install
npm i vue-lazy-load

// 使用插件
import Vue from 'vue'
import VueLazyLoad from 'vue-lazy-load'
Vue.use(lazyload, {
	loading: 'default/path/to/file.png',
	error: 'error/path/to/file1.png'
})

// 使用v-lazy指令
<div v-for="img in list">
	<img v-lazy="img">
</div>
```


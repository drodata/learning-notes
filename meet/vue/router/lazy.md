# Lazy loading Routes

预备知识：

- Vue.js 的 async components 特性；
- Webpack 的代码分割特性；

通常情况下，我们在配置路由时，都是直接引用一个组件，例如：

```js
// entry script
import Demo from './components/Demo'

router.map({
  '/demo': {
    component: Demo
  }
}
```

随着路由个数的增加， entry script 顶部就会出现一长串 `import` 语句。Vue.js 提供了一种动态加载组件的特性—— Async components. 也就是说，仅当路由 `/demo` 需要被显示时， `Demo` 组件才会异步加载进来。

使用惰性加载的方式加载 `Demo` 组件的代码如下：

```js
// entry script
router.map({
  '/demo': {
    component: function (resolve) {
      require(['./components/Demo'], resolve)
    }
  }
}
```

可以看到，我们不必在 entry script 顶部引用 `Demo` 组件。通过 lazy loading 技术，仅当用户访问 `/demo` 页面时， `Demo` 以及它的依赖组件才会异步地加载进来。

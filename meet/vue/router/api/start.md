# `router.start(App, el, [callback])`

新建一个 `App` 实例，并挂载到 `el` 元素上。

## 参数

### Function | Object `App`

`App` 可以是一个 Vue component constructor 或者一个 component options object. 如果是后者，the router 会隐形调用 `Vue.extend`. 这个组件用来为单页应用创建一个 root Vue instance.

Note: vue-router cannot be started with Vue instances.

```js
const App = Vue.extend(require('./app.vue'))
router.start(App, '#app')
```

### String | Element `el`

即可以是一个 CSS selector, 也可以是 _actual element_.

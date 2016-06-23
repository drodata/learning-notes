# Route Options

还记得我们在 `main.js` 中创建的 `router` 实例吗？

```js
var router = new VueRouter()
```

当时我们为了简单起见，没有设置任何参数。Vue-router 提供了很多参数，让我们可以在创建 router 实例时，改变 router 默认的行为。可用的 options 如下：

名称    | 默认值    | 使用范围  |  说明
--------|-----------|-----------|------
`hashbang` | `true` | 仅用于 hash mode | 例如，`router.go('/foo/bar')` 会将 URL 设置为 `a.com/#!/foo/bar`
`history` | `false` | | 开启后，可使用 H5 history API 管理浏览器历史记录
`abstract` | `false` | | TBD
`root` | `null` | 仅用于 history mode | 
`saveScrollPosition` | `false` | 仅用于 history mode | 自定义 active class name 
`linkActiveClass` | `v-link-active` | | 自定义 active class name 
`transitionOnLoad` | `false` | | TBD
`suppressTransitionError` | `false` | | 开启后，uncaught errors inside transition hooks 将会被抑制

Vue-router repository 自带了一个 advanced example, 它使用的 options 如下：

```js
const router = new VueRouter({
  history: true,
  saveScrollPosition: true
})
```

这表示启用 history mode, 最直观的变化是： url 中的 hashbang (`#!`) 不见了。

参考：

- http://diveintohtml5.info/history.html HTML5 history API 入门文章。如果两个页面几乎完全相似，进行页面跳转时，没必要完整加载新页面，借助 HTML5 history API, 只需将不同的内容通过 AJAX 的方式将原内容替换掉（swap），同时修改 url 地址。对用户来说，看不出区别，但大大节省了时间和传输成本。

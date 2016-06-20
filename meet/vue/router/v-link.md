# v-link

该指令符接受的值是一个 JavaScript 表达式。例如：

```vue
<a v-link="'home'">Home</a>

<a v-link="{ path: 'home' }">Home</a>

<a v-link="{ name: 'user', params: { userId: 123 }}">User</a>
```

在内部，该表达式作为参数传递给 `router.go()`.

## `v-link` 相比硬编码属性 `href` 的优势

TBD

## Active Link Class

当带有 `v-link` 指令符的元素（例如一个链接）匹配当前路径时，该元素会自动增加一个类名。默认类名是 `.v-link-active`.

例子：假设有下面一个链接：

```vue
<a v-link="{path: '/demo'}">Demo</a>
```

如果当前路径是 `/demo`, 我们在控制台会看到该链接的完整样式为：

```html
<a _v-60278adf="" href="#!/demo" class="v-link-active">Demo</a>
```

### 默认匹配原则—— Inclusive Match 和精确匹配

假设一个元素的 `v-link` 值为 `/a`, 那么，只要，路径以 `/a` 开头（`/a/b`, `a/goo` 等），active class 都会应用到该元素上。

我们自然会想，如何能实现精确匹配？ 答案是使用 `v-link` 的 `exact` inline option:

```vue
<a v-link="{path: '/a', exact: true}">A</a>
```

加上 `exact` option 后，active class 进当路径为 `/a` 时，才会应用到上面的链接上。

### 更改默认的 Active Class Name

方法一：使用 `linkActiveClass` [route option](/meet/vue/router/option.md).

```js
// main.js
var router = new VueRouter({
  linkActiveClass: 'red'
})
```

方法二： 使用 `v-link` 的 `activeClass` inline option:

```vue
<a v-link="{path: '/a', activeClass: 'red'}">A</a>
```

### 将 Active class 应用到其它元素上

假设 template 如下：

```vue
<ul>
  <li>
    <a v-link="{ path: '/xxx' }">Go</a>
  </li>
</ul>
```

当路径为 `/xxx` 时，我想让 active class 应用到链接的 wrapping 元素 `<li>` 上。只需在 wrapping 元素上添加 `v-link-active` 指令符即可：

```vue
<ul>
  <li v-link-active>
    <a v-link="{ path: '/xxx' }">Go</a>
  </li>
</ul>
```

## Inline Configuration Options 配置选项

前面我们已经介绍了 `exact` 和 `activeClass` 两个 inline options. 还有两个：

- boolean `replace` (默认值为 `false`):
  
  若此选项值为 `true`, 当点击链接时，将使用 `router.replace()` 而不是 `router.go()`. 两者的区别是：`router.replace()` 不会留下一条历史记录。实例：
  
  ```vue
  <a v-link="{ path: '/abc', replace: true }"></a>
  ```

- boolean `append` (默认值为 `false`):
  
  此选项值为 `true` 的相对链接，在跳转时，会将相对链接追加到当前路径后面。举个例子，假设我们从路径 `/a` 跳转到一个相对链接 `b`, 不配置 `append: true` 的话，我们会跳转到 `/b`; 而配置 `append: true` 的话，将跳转到 `/a/b`. 换句话说，相对路径 `b` 被附加到了当前路径 `/a/` 后面。
  
  ```vue
  <a v-link="{ path: 'b/c', replace: true }"></a>
  ```

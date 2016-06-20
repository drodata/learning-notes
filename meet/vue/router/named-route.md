# Named Routes 具名路由

那些含有 dynamic segment 或 star segment 的路由最适合设置成具名路由,  为它赋予一个名称，可以很方便地创建链接。

## 如何命名

```js
router.map({
  '/user/:userId': {
    name: 'user',
    component: { ... }
  }
})
```

## Linking to a named route

有两种途径：一是在 template 内使用 `v-link` directive:

```vue
<a v-link="{ name: 'user', params: { userId: 123 }}">User</a>
```

另一种方法是使用 `router.go()`:

```js
router.go({ name: 'user', params: { userId: 123 }})
```



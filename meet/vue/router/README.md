# Vue-router

1. [指令符 `v-link`](/meet/vue/router/v-link.md)
    - `v-link` 指令符的本质是调用 `router.go()` api;
    - 接受的值是一个 JavaScript 表达式，例如：`{ path: 'home'}`, `{ name: 'user', params: {id: 123}}` 或者 `'home'` (literal string);
    - Active class: 如果 `v-link` 指定的路由匹配到当前路径，默认将应用 `.v-link-active` 类到该链接；
    - 4 个 inline options: `exact`, `activeClass`, `replace` 和 `append`. 这 4 个选项中，除了 `activeClass` 的值是 string 外，其它三个均为 boolean;

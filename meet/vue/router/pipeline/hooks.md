# Transition Hooks

> 预备知识
>
> [Promise](/es6/promise.md)

`<router-view>` 组件可以通过一系列钩子函数来控制管道切换。这些钩子函数包括：

- `data`
- ...

可以通过组件的 `route` option 来实现（implement）这些钩子函数。

```vue
<script>
export default {
  route: {
    activate: function () {
      console.log('hook-example activated!')
    },
    canDeactivate: function (transition) {
      console.log('You are not allowed to leave.')
      transition.abort()
    }
  },
  data() {
    return {
      message: 'hello'
    }
  }
}
</script>
```
## 钩子函数中的唯一参数 `transition`

它是一个对象，有如下属性或方法：

- `from`
- `to`
- `next()`
- `abort([reason])`
- `redirect(path)`

## Hook Resolution Rules 钩子函数异步 resolve 规则

> - We often need to perform asynchronous tasks inside transition hooks.
> - The transition will not proceed until an asynchronous hook has been resolved. 
> - Here are the rules to determine when a hook should be considered resolved:

1. 若钩子函数返回的是一个 Promise, 则钩子函数将在 Promise 被 resolve 后再 resolve
2. 没有返回一个 Promise, 且钩子函数不带任何参数，它将被同步 resolve.
3. 没有返回一个 Promise, 但携带 `transition` 参数，那么，只有在 transition.next()`, `transition.abort()` 或 `transition.redirect()` 被调用时才会被 resolve.
4. 在验证类钩子 `canActivate`, `canDeactivate` 以及 global `beforEach` hook 中，若返回值是布尔类型，也会使得钩子函数被 resolve

## 在钩子函数内返回 Promise

- Promise 被成功 resolve 后，会自动调用 `transiton.next()`;
- 若 Promise 在验证阶段被 reject, 将调用 `transiton.abort()`;
- 若 Promise 在激活阶段被 reject, 将调用 `transiton.next()`;
- 对验证类钩子函数来说，if the Promise's resolved value is falsy (假值), 它将中止切换；
- If a rejected promise has an uncaught error, it will be thrown

实例：

```js
// inside component definition
route: {
  canActivate: function () {
    // assuming the service returns a Promise that
    // resolve to either `true` or `false`.
    return authenticationService.isLoggedIn()
  },
  activate: function (transition) {
    return messageService
      .fetch(transition.to.params.messageId)
      .then((message) => {
        // set the data once it arrives.
        // the component will not display until this
        // is done.
        this.message = message
      })
  }
}
```

借助 ES6 中的 argument destructuring, 可以让钩子看起来更简洁：

```js
route: {
  activate ({ next }) {
    // when done:
    next()
  }
}
```

Vue-router 中内置了一个 [advanced exaple](https://github.com/vuejs/vue-router/tree/dev/example/advanced), 有助于更好地理解本节内容。

## 钩子合并

TBD

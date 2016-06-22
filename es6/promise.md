# Promise

## 快速入门

Promise 实例生成以后，可以使用 `then` 方法分别指定 `Resolved` 和 `Rejected` 状态的回调函数：

```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

- 第 2 个参数可选；
- 两个函数的参数 `value` 和 `error` 都是 Promise 实例传出的值。

## 简单例子

```js
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(3000).then(function (value) {
  console.log(value);
}, function (error) {
  console.log(error);
})
```

Ref.

- https://github.com/ruanyf/es6tutorial/blob/gh-pages/docs/promise.md


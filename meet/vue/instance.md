# Vue 实例

## 构造器

每一个 Vue.js 应用的起步都是通过构造函数 `Vue` 创建一个 Vue 根实例：

```js
var vm = new Vue({
  // 选项
})
```

## 属性（代理属性与实例属性）与方法

每个 Vue 实例都会代理其 `data` 对象里的所有属性，例如：

```js
var data = { a: 1 }
var vm = new Vue({
  data: data
})

vm.a === data.a // -> true

// 设置属性也会影响到原始数据
vm.a = 2
data.a // -> 2

// ... 反之亦然
data.a = 3
vm.a // -> 3
```

除了代理属性外， Vue 实例还提供了一些有用的实例属性与方法，这些属性或方法都有前缀 `$`, 以便和代理属性区分开来，例如：

- 所有实例属性列表： http://cn.vuejs.org/api/#实例属性
- 所有实例方法： http://cn.vuejs.org/api/#实例方法-数据

## 实例生命周期

所有生命周期钩子函数列表：

- `init`
- `created`
- `beforeCompile`
- `compiled`
- `ready`
- `attached`
- `detached`
- `beforeDestroy`
- `destroyed`

详见：http://cn.vuejs.org/api/#选项-生命周期钩子

## 实例生命周期图示

![](http://cn.vuejs.org/images/lifecycle.png)

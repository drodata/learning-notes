# 计算属性 Computed Properties

我们在[数据绑定语法](/meet/vue/syntax.md)中已经讲过，绑定表达式只能是单个表达式。这是为了保持模板文件内的清洁（仅含有视图结构，不包含数据逻辑代码）。如果需要多于一个表达式的逻辑，应当使用计算属性了。

## Basic Example

```html
<div id="example">
  a={{ a }}, double a ={{ aDouble }}
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    a: 1
  },
  computed: {
    // 一个计算属性的 getter
    aDouble: function () {
      // `this` 指向 vm 实例
      return this.a * 2
    }
  }
})
```

这里我们声明了一个计算属性 `aDouble`. 我们提供的函数用作属性 `vm.aDouble` 的 getter 函数.

```js
console.log(vm.aDouble) // -> 2
vm.a = 2
console.log(vm.aDouble) // -> 4
```

代理属性 `vm.aDouble` 和 `vm.a` 之间建立了依赖：前者的值始终取决于后者。

> the best part is that we’ve created this dependency relationship **declaratively**: the computed getter function is pure and has no side effects, which makes it easy to test and reason about.
> 
> 而且最妙的是我们是声明式地创建这种依赖关系：计算属性的 getter 是干净无副作用的，因此也是易于测试和理解的。

TBD: 上面的话理解不了。

## 计算属性 V.S. `$watch` 实例方法

## 计算 setter

计算属性默认只是 getter, 不过我们可以在需要时提供一个 setter:

```js
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```

现在调用 `vm.fullName = 'Kui Chen'` 时，setter 函数会被调用，而且 `vm.firstName` 和 `vm.lastName` 的值也会相应进行更新。

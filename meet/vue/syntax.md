# 数据绑定语法

## 插值 Interpolations
### Text

文本插值是数据绑定最常见的形式，使用双大括号语法：

```vue
<p>Message: {{ msg }}</p>
```

`{{ msg }}` 会被对应的数据对象的 `msg` 属性值替换。当该属性值发生变化时， `{{ msg }}` 会自动更新值。

若你不想自动更新值（单次插值），使用下面的语法：

```vue
<p>Message: {{ * msg }}</p>
```

### Raw HTML `{{{ row_HTML }}}`

三个大括号表示直接输出原始的 HTML.

### HTML Attributes

双大括号语法也可以用在 HTML 属性内：

```
<div id="item-{{ id }}"></div>
```

在 Vue.js 内部，所有 HTML 属性插值都会转换成 `v-bind` 指令。

## 绑定表达式 Binding Expressions

放在双大括号内的文本被成为_绑定表达式_. 在 Vue.js 中，一段绑定表达式由一个简单的 JavaScript 表达式和可选的一个或多个过滤器构成。在前面的例子中，`msg` 和 `id` 都叫绑定表达式。

此外，指令的值也是绑定表达式。

### JavaScript Expressions

必须是单个表达式。例如，下面的表达式合法：

```vue
{{ msg }}

{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}
```

而下面的表达式非法：

```vue
<!-- 这是一个语句，不是一个表达式： -->
{{ var a = 1 }}

<!-- 流程控制也不可以，可改用三元表达式 -->
{{ if (ok) { return message } }}
```

之所以有此限制，是因为模板文件（`<template>` 里的内容）的主要作用是描述试图的结构，加入过多的逻辑内容，会让模板文件变得臃肿且难于维护。

### Filters 

表达式后面可以添加可选的过滤器，以 `|` 连接，例如：

```vue
{{ message | capitalize }}
```

过滤器的本质是一个 JavaScript 函数。上面的 `capitalize` filter 的作用是首字母大写。加入 `message` 的值是 `hello`, 那么模板最终输出内容为 `Hello`.

#### 过滤器参数 Filters Arguments

```vue
{{ message | filterA 'arg1' arg2 }}
```

- 过滤器函数的第一个参数永远是表达式的值；
- 带引号的参数视为字符串；
- 不带引号的参数则视为表达式；

## 指令 Directives

参见 directive 内容。

### Arguments
### Modifiers

## 缩写

对于常用的指令，使用完整的 `v-*` 写法太罗嗦。Vue.js 针对最常用的两个指令 `v-bind` 和 `v-on`, 分别指定了缩写形式。

### `v-bind` Shorthands `:`

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>

<!-- 缩写 -->
<a :href="url"></a>

<!-- 完整语法 -->
<button v-bind:disabled="someDynamicCondition">Button</button>

<!-- 缩写 -->
<button :disabled="someDynamicCondition">Button</button>
```
### `v-on` Shorthands `@`

```vue
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>

<!-- 缩写 -->
<a @click="doSomething"></a>
```

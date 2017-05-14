# 模板

## Defining via `name` attribute

`name` 属性值是模板名称，在 `<template/>` 内定义代码片段：

```xml
<template name="msgItem">
    <view>
        <text> {{index}}: {{msg}} </text>
        <text> Time: {{time}} </text>
    </view>
</template>
```
## Using via `is` attribute

模板所需的数据通过 `data` 属性传入：

```xml
<template is="msgItem" data="{{...item}}"/>
```

```js
Page({
    data: {
        item: {
            index: 0,
            msg: 'this is a template',
            time: '2016-09-15'
        }
    }
})
```

## 动态渲染

`is` 属性可以使用 mustache 语法，来动态决定渲染哪个模板：

```xml
<template name="odd">
    <view> odd </view>
</template>
<template name="even">
    <view> even </view>
</template>

<block wx:for="{{[1, 2, 3, 4, 5]}}">
    <template is="{{item % 2 == 0 ? 'even' : 'odd'}}"/>
</block>
```

## 作用域

模板拥有自己的作用域，只能使用 `data` 传入的数据。

# API 界面

## 交互反馈

### Show / Hide Toast

Signature `wx.showToast(Object object)`. `object` 参数：

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
title | String | Y | | 提示内容
icon | String | N | | allowd values: 'success', 'loading'
image | String | N | | 自定义图标的本地路径， image 优先级高于 icon
duration | Number | N | 1500 | 提示的延迟时间，单位毫秒
mask | Boolean | N | false | 是否显示遮罩层，防止触摸穿透

Example:

```js
wx.showToast({
    title: '成功',
    icon: 'success',
    duration: 2000
})

wx.hideToast()
```
### Show / Hide Loading

Signature `wx.showLoading(Object object)`. `object` 参数：

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
title | String | Y | | 提示内容
mask | Boolean | N | false | 是否显示遮罩层，防止触摸穿透

Example:

```js
wx.showLoading({
    title: 'Processing...',
})

setTimeout(function(){
    wx.hideLoading()
},2000)
```

### Show Modal `wx.showModal(Object object)`

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
title | String | Y | | 
content | String | Y | | 
confirmText | String | N | '确定' | 至多 4 个字符
confirmColor | HexColor | N | '#3CC51F' | 确定文字颜色
showCancel | Boolean | N | true | 是否显示取消按钮
cancelText | String | N | '取消' | 至多 4 个字符
cancelColor | HexColor | N | '#000000' | 取消文字颜色

success 回调函数可用参数表

参数 | 类型  | 说明
-----|-------|------
confirm | Boolean | 值为 true 时表示用户点击了确认按钮
cancel | Boolean | 值为 true 时表示用户点击了取消按钮（用于 Android 系统中区分点击蒙层关闭还是点击按钮关闭）

```js
wx.showModal({
    title: 'Confirm',
    content: 'Please confirm deletion',
    confirmText: 'Delete',
    success: function(res) {
        if (res.confirm) {
            // do deletion
        } else if (res.cancel) {
            console.log('User cancel deletion.')
        }
    }
})
```

### Show ActionSheet `wx.showActionSheet(Object object)`

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
itemList | String Array | Y | | 至多 6 个,例如 `['A', 'B', 'C']`
itemColor | HexColor | N | '#000000'| 按钮文字颜色

success 回调函数可用参数表

参数 | 类型  | 说明
-----|-------|------
tapIndex | Number | 选中按钮 index 值，从 0 开始

```js
wx.showActionSheet({
    itemList: ['A', 'B', 'C'],
    success: function(res) {
        console.log(res.tapIndex)
    },
    fail: function(res) {
        console.log(res.errMsg)
    }
})
```

## 页面顶部导航条

### `wx.setNavigationBarTitle(Object object)` 动态设置页面标题

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
title | String | Y | | 页面标题

```js
wx.setNavigationBarTitle({
    title: 'Package Sending'
})

wx.showNavigationBarLoading()

wx.hideNavigationBarLoading()
```

## 导航

### `wx.navigateTo(Object object)`, `wx.navigateTo(Object object)`

我们将这两个函数放在一起记忆： 

- navigateTo() 保留当前页面，跳转到应用内的某个页面，使用 `wx.navigateBack()` 可以返回到原页面；
- redirectTo() 关闭当前页面并跳转；
- :bell: navigate 和 redirect 不允许跳转到 tabbar 页面，只能用 wx.switchTab 跳转到 tabbar 页面

两个函数使用相同的参数：

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
url | String | Y | | 需要跳转的非 tabBar 页面地址，可以携带 query string

```js
wx.navigateTo({
    url: 'order?id=1'
})


// order.js
Page({
    onLoad: function(option){
        console.log(option.query) // 'id=1'
    }
})

// redirect
wx.redirectTo({
    url: 'test?id=1'
})
```

### `wx.navigateBack(Object object)`

关闭当前页面，返回上一页或多级页面。

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
delta | Number | N | 1 | 返回页面数，若值大于现有页面数，则返回至首页

:bell: 调用 `navigateTo` 跳转时，调用该方法的页面会被加入堆栈，而 redirectTo 方法则不会。

```js
// in page a
wx.navigateTo({
    url: 'page-b?id=1'
})

// in page b
wx.navigateTo({
    url: 'page-c?id=1'
})

// return to page a in page c
wx.navigateBack({
    delta: 2
})
```

### `wx.switchTab(Object object)` Tab 间跳转

跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面。

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
url | String | Y |  | 此路径必须在 `app.json` 中的 `tabBar` 属性中预先定义，路径后**不能带参数**

```js
// in app.json
{
    "tabBar": {
        "list": [
            {
                "pagePath": "index",
                "text": "Home"
            },
            {
                "pagePath": "user",
                "text": "User"
            }
        ]
    }
}

// in a page
wx.switchTab({
    url: '/user'
})
```

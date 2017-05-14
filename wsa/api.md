# API

## 共性

共有的参数：

参数 | 类型 | 说明
-----|------|------
success | Function | 成功则返回图片的本地文件路径列表 tempFilePaths
fail | Function | 接口调用失败后的回调函数
complete | Function| 接口调用结束的回调函数（不管成功或失败，都会执行，和 jQuery 中的 always 类似）

多数情况下，上面三个参数都是可选的，只有 `wx.chooseImage()` 的 `success` 是必填参数。因此，下面的 API 参数表中，我们一律不显示这三个参数，除非它是必须参数。

## Network

### `wx.request(Object)` 发起 HTTPS 请求

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
url | String | Y | | 开发者服务器 url
data | String, Object | N | | 
header | Object | N | | 
method | String | N | 'GET'| 注意：**必须大写**
dataType | String | N | 'json' | 

- `data` 额外说明：最终发送给服务器的是 String 类型，若传入的数据类型是 Object, 则会被转换成字符串，转换规则如下：
    - `header['content-type']` 为 `'application/json'` 的数据，会对数据进行 JSON 序列化
    -

Example:

```js
wx.request({
    url: 'https://i.example.com/order/create',
    data: {
        name: 'hope' ,
        password: 'something'
    },
    header: {
        'content-type': 'application/json'
    },
    success: function(res) {
        console.log(res.data)
    }
})
```

### `wx.uploadFile(Object)`

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
url | String | Y | | 开发者服务器 url
filePath | String | Y | | 要上传文件的路径
name | String | Y | | 文件对应的 key , 开发者在服务器端通过这个 key 可以获取到文件二进制内容
header | Object | N | | 
formData | Object | N | | HTTP 请求中其它额外表单数据

Success 返回参数：

参数 | 类型 | 说明
-----|------|------
data | String | 开发者服务器返回的数据
statusCode | Number |


Example (Choose + upload):

```php
wx.chooseImage({
    success: function(res) {
        var tempFilePaths = res.tempFilePaths

        wx.uploadFile({
            url: 'http://example.com/upload', 
            filePath: tempFilePaths[0],
            name: 'file',
            formData:{
                'orderId': 535
            },
            success: function(res){
                var data = res.data
                //do something
            }
        })
    }
})
```

## Media

### Choose Image `wx.chooseImage(Object)`

Object 参数表

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
success | Function | Y | | 成功则返回图片的本地文件路径列表 tempFilePaths
count | Number | N | 9 | 最多可选图片张数
sizeType | String, Array | N | ['original', 'compressed'] | original, compressed
sourceType | String, Array | N | ['album', 'camera'] | album, camera

Example:

```php
wx.chooseImage({
    count: 1,
    sizeType: ['compressed'],
    success: function (res) {
        // tempFilePath可以作为img标签的src属性显示图片
        var tempFilePaths = res.tempFilePaths
    }
});
```

## Local Storage 本地缓存

基本要点：

- 每个微信小程序都可以有自己的本地缓存，可以通过相关 API 对本地缓存进行设置、获取和清理。
- 同一个微信用户，同一个小程序 storage 上限为 10MB。
- localStorage 以用户维度隔离，同一台设备上，A 用户无法读取到 B 用户的数据。
- :warning: localStorage 是永久存储的，但是我们不建议将关键信息全部存在 localStorage，以防用户换设备的情况。
- 本地缓存共有 10 个函数，实际上只有 5 个，每一个都有对应的同步函数，函数名有一个 `Sync` 后缀，所以下面的函数将他们分成 5 组记录。
    - 异步函数接受的参数类型是 Object, 同步函数接受的参数是两个：key 和 data;

###  `wx.setStorage()`, `wx.setStorageSync()`

将数据存储（若已存在，将覆盖）在本地缓存中指定的 key 中。

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
key | String | Y | | key name
data | String, Object | N | | key value

Example:

```js
// async version
wx.setStorage({
    key: "username",
    data: "jack"
})

// sync version
try {
    wx.setStorageSync('key', 'value')
} catch (e) {
}
```

### `wx.getStorage(Object)`, `wx.getStorageSync(String key)`

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
key | String | Y | | key name
success | Function | Y | | 回调函数，`res.data` 是获得的 key 值

```js
// async version
wx.getStorage({
    key: 'key',
    success: function(res) {
        console.log(res.data)
    } 
})

// sync version
try {
    var value = wx.getStorageSync('key')
    if (value) {
        // real code
    } 
} catch (e) {
    // error handling
}
```

### `wx.getStorageInfo(Object)`, `wx.getStorageInfoSync(Object)`

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
success | Function | Y | | 可用的参数详见下方的表格

success 回调函数参数

参数 | 类型 | 说明
-----|------|------
keys | String Array | 当前 storage 中所有 keys
currentSize | Number | 当前占用空间，单位 kb
limitSize | Number | 限制的空间大小，单位 kb

```js
// async version
wx.getStorageInfo({
    success: function(res) {
        console.log(res)
    }
})

// sync version
try {
    var res = wx.getStorageInfoSync()
    console.log(res.keys)
} catch (e) {
    // error handling
}
```

### `wx.removeStorage(Object object)`, `wx.removeStorageSync(String key)`

从本地缓存中删除指定 key.

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
key | String | Y | | key name
success | Function | Y | |

```js
// async version
wx.removeStorage({
    key: 'key',
    success: function(res) {
        console.log(res.data)
    } 
})

// sync version
try {
    wx.removeStorageSync('key')
} catch (e) {
    // error handling
}
```

### `wx.clearStorage()`, `wx.clearStorageSync()`

（同步）清空本地缓存。

```js
wx.clearStorage()

try {
    wx.clearStorageSync()
} catch(e) {
    // ...
}
```

## Device

### `wx.scanCode(Object object)`

调起客户端扫码界面，扫码成功后返回对应的结果。

success 回调函数可用参数：

参数 | 类型 | 说明
-----|------|------
result | | 扫码内容
scanType | | 扫码类型
charSet || 所扫码字符集
path || 当所扫的码为当前小程序的合法二维码时，会返回此字段，内容为二维码携带的 path

```js
wx.scanCode({
    success: (res) => {
        console.log(res)
    }
})
```

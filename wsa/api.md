# API

## Network

### `wx.request(Object)` 发起 HTTPS 请求

参数 | 类型 | 必填 | 默认值 | 说明
-----|------|------|--------|------
url | String | Y | | 开发者服务器 url
data | String, Object | N | | 
header | Object | N | | 
method | String | N | 'GET'| 注意：**必须大写**
dataType | String | N | 'json' | 
success | Function | N | | 
fail | Function | N ||
complete | Function | N ||

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
success | Function | N | | 
fail | Function | N ||
complete | Function | N ||

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
fail | Function | N || 接口调用失败后的回调函数
complete | Function | N || 接口调用结束的回调函数（不管成功或失败，都会执行，和 jQuery 中的 always 类似）
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

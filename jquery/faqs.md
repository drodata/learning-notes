# FAQs

### 如何判断一个 JavaScript 变量是否存在（定义过）？

```js
if ( typeof( var ) != 'undefined')
```

jQuery 变量判断

:warning: 判断使用 `.find()` 的返回值可**不能使用此方法**，因为不管是否找到，`.find()` 的返回值类型都是 object, 因为 `.find()` 的返回值是 `jQuey`, 正确的方法是：

```js
if ($.find('#a').length >0) {}
```

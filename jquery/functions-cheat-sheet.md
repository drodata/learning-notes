# Funtions Cheat-sheet

## JavaScript Native Functions

- `parseInt()`, `parseFloat()`: 例如: `parseInt('1')`
- `Math.round()`
- `isNaN()`: 判断一个变量值是否是 `NaN`;
- `.toFixed(number)`: 将一个浮点数只保留小数点后 `number` 位，例子：
  
  ```js
  var amt = parseFloat( $('#quantity').html() );
  amt.toFixed(2);
  ```

## jQuery Utilities

### `$.now()`: 返回当前 timestamp
  
  用例：浏览器有缓存，所以放问有的静态文件（如程序生成的 PDF 文档）时, 可以在借助此函数在 url 后面加上一个不断变化的字符串。

### `$.inArray()`: PHP `in_array()` 函数的 jQuery 版

:warning: 与 PHP 的 `in_array()` 不同的是，`$.inArray()` 的返回值是元素的索引值（如果找到）或 -1 (找不到)，而不是布尔类型的“是否找到”：

> Search for a specified value within an array and **return its index (or -1 if not found)**.

因此，使用该函数判断一个元素是否在数组内的正确写法是：

```js
if ($.inArray(3, ['3', 'name']) > -1) {
}
```

上面的代码的 if 判断**将返回 `-1`**, 这是因为采用严格匹配（类型也必须相同）的方式。

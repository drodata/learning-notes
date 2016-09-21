# Selectors

- http://api.jquery.com/category/selectors/

## Attribute Equals Selector [name=”value”]

- `attribute`: An attribute name.
- `value`: An attribute value. Can be either **an unquoted single word** or a **quoted string**.
  
  :warning: 不带引号**仅适用于 _single word_**.

*single word* 范例：

```js
$("input[name='Order\\[currency\\]']")

// also OK, matches <input name="order" />
$("input[name=order]")
```

错误的写法实例：

```js
$("input[name=Order\\[currency\\]]")
```

该写法将抛出异常：

> Uncaught Error: Syntax error, unrecognized expression: `input[name$=currency]]`

- `[name^=”value”]`, `[name$=”value”]`: 属性值以 `value` 开头/结尾（精确匹配，区分大小写）的元素。
  
  例子：`$('input[name$="unit_price\\]"]')`. Yii2 的 tabular input 中生成的元素名称前面有 index, 后面相同，最适合用 `$` selector;
- Child Selector: `("parent > child")`. 
  
  ```
  $('select > option')
  ```
- Descendant Selector: `(“ancestor descendant”)`. 这里的 descendant 不限于 child, 还可以是孙子、重孙等；
  
  ```
  $('table td')
  ```
- Multiple Selector: `("selector1, selector2, selector 3")`.
  
  将零散的多个元素组合在一起做相同的操作。例子：新建客户时，如果是国外客户，则需要将 province, city 两 fields 同时 slide up, 使用此 selector 只需一条语句即可；

- https://trello.com/c/SqsGFUnj
  
  ```
  $('[name=province]')
  $("[name='Order\\[province\\]']")
  ```


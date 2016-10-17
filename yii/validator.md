# Validators

## Conditional Validation `when` and `whenClient`

假如想在满足某个条件时才进行属性验证，可以使用 `when` property. `when` 的值是一个 callback, signature 为：

```js
/**
 * $model: 当前验证的 model
 * $attribute: 当前验证的属性
 */
function ($model, $attribute) { }
```

例子：

```php
[
    // 仅当国家为中国时，省市区三列才进行 required 验证
    ['province_id', 'city_id', 'district_id'],
    'required',
    'when' => function($model) {
        return $model->country == 'CHN';
    }
]
```

### Client-Side Conditional Validation `whenClient`

`when` property **仅在服务器端**生效。通过配置 `whenClient` property, 可以实现客户端验证。

`whenClient` 的值是用字符串表示的 JavaScript 函数，其 signature 为：

```js
function (attribute, value) { }
```

其中 `value` 表示属性的当前值， `attribute` 是一个对象，包含如下属性：

- `id:` a unique ID **identifying the attribute** (e.g. `loginform-username`) in the form
- `name:` **attribute name** or expression (e.g. `[0]content` for tabular input)
- `container:` the jQuery selector of the **container of the input field**
- `input:` the jQuery selector of the **input field** under the context of the form
- `error:` the jQuery selector of the error tag under the context of the container
- `status:` status of the input field, 0: empty, not entered before, 1: validated, 2: pending validation, 3: validating

上面那个省市区验证用 `whenClient` 写法为：

```js
'whenClient' => "function (attribute, value) {
    return $('#country').val() == 'USA';
}"
```

Ref. http://www.yiiframework.com/doc-2.0/guide-input-validation.html#conditional-validation

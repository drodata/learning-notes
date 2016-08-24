# Yii FAQs

## 如何在 AR 对象内获取 Controller 对象？

我需要在 model 内获取一个 View 的内容并存入一个字符串内。在 controller 内，我们可以使用 `$this->renderPartial()` 方法。在 model 内， View object 可通过如下方式获得：

```php
$controller = Yii::$app->controller;
$str = $controller->renderPartial(['/order/view', 'id' => 3]);
```

容易搞错的是，`render()` 是 view 对象内的方法；`renderPartial()` 是 controller 对象内的方法。

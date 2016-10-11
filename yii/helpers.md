# `ArrayHelper`

## `merge( $a, $b )`

将两个数组合并成一个数组。两个数组中若出现同一个元素，则**后者覆盖前者**。该方法最常用的例子莫过于 `index.php` 中将多个配置数组合并成一个了。


## `remove()` & `getValue()`

```php
mixed|null remove ( &$array, $key, $default = null )
mixed getValue ( $array, $key, $default = null )
```

相同点：都是从数组中取值；不同点：`getValue()` 取值后原数组不变，`remove()` 中 `$array` 前面的 `&`, 这表示 `$array` 取一个少一个。

## `map()`

```php
public static array map ( $array, $from, $to, $group = null )
```

例子：

```php
ArrayHelper::map(
    User::find()->where(['status' => 1])->asArray()->all(),
    'id',
    'username',
    'group_id'
);
```

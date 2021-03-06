# PHP FAQs

## 如何使用 `&&` 或 `||` 操作符实现短路写法


### 例一

假设一个产品有多张产品图片，在做产品导入时，需要判断一张图片是否已经在系统中存在，若存在，则不再重复上传。这个逻辑怎么写比较好呢？

方法一：借助 `in_array()` 来判断：

```php
$hashes = [];
foreach ($this->images as $image) {
    $hashes[] = md5_file($image->path);
}
return !in_array(md5_file($imagePath), $hashes);
```

方法二：使用 `&&` 操作符

```php
$status = true;
foreach ($this->images as $image) {
    $status = (md5_file($image->path) != md5_file($imagePath)) && $status;
}
return $status;
```

显然，后者更简洁些。

### 例二

假设有个产品表，许多其它表（订单表、库存表等）都将产品表 ID 作为外键。在删除一条产品表记录时，我们需要先逐一检查在其它表中，是否存在包含该记录的外键约束，是则不能直接删除。借助 `||` 操作符，这个判断的方法可以这么写：

```php
public function hasConstraint()
{
    $flag = false;
    $modelNames = ['Stock', 'Item', 'InventoryItem', 'PurchaseItem'];
    foreach ($modelNames as $modelName) {
        $modelName = "\\backend\\models\\" . $modelName;
        $record = $modelName::findOne(['product_id' => $this->id]);
        $flag = !is_null($record) || $flag; // 关键
    }
    return $flag;
}
```

`static` V.S. `self`
-------------------------------

### Yii2 Coding Style 中

绝大部分都要使用 `static`, 只有下面三种特殊情况使用 `self`:

- 访问常量时：`self::UNPAID`;
- 访问 _**private static properties**_: `self::$_events`;
- _It is allowed to use self for method calls where it makes sense such as recursive call to current implementation instead of extending classes implementation._

### `static` 关键字有三种使用场合

- 在类**内部**声明静态属性、方法
  
  ```php
  class Order
  {
      public static $name;
  
      public static function latest()
      {
      }
  }
  ```

- 在类方法内部使用 static::xx 调用静态方法
- 在函数内部声明静态变量

`empty()`, `isset()`, `is_null()` 联系
-------------------------------

> A variable is considered ***empty*** if it does not exist or if **its value equals `FALSE`**.

`empty($var)` 等价于 `!isset($var) || $var == false`.


> `isset()`: Determine if a variable is set and is not `NULL`.

### 变量被认为 *empty*  或 FALSE 的情况

- "" (an empty string)
- 0 (0 as an integer)
- 0.0 (0 as a float)
- "0" (0 as a string) :warning: `empty("0.00")` 的值是 `false`; 
- NULL
- FALSE
- array() (an empty array)
- $var; (a variable declared, but without a value)

从中看到，NULL 和 空数组被视为 *empty*, 考虑到 Yii AR classes 类似 findOne(), findAll() 方法中，找不到记录的返回值是 null 或 empty array, 因此，使用 `empty()` 进行判断最好不过，例如：

Yii2 core coding style 中讲到一条关于 `empty()` 的用法：


> `=== []` vs `empty()`
> 
> Use `empty()` where possible.
> 
> https://github.com/yiisoft/yii2/blob/master/docs/internals/core-code-style.md#--vs-empty

```
$records = Order::findAll(['status' => 1]);
if (empty($records)) {
	return false;
}
foreach ($records as $r) {
	// ...
}
```

### 变量被认为 NULL的情况

- 被赋值为 `null`;
- 仅声明，没有赋值；
- 通过 `unset()` 函数显性 unset;

### Is `"0.000"` Empty?

假设 `order` 有一列 `rate` 来存储汇率，该列类型为：

```
+----------+-----------------------+------+-----+---------+-------+
| Field    | Type                  | Null | Key | Default | Extra |
+----------+-----------------------+------+-----+---------+-------+
| rate     | decimal(8,4) unsigned | NO   |     | NULL    |       |
+----------+-----------------------+------+-----+---------+-------+
```

如果新建一个订单，`$order->rate` 的值将是**字符串类型** `0.0000`: 

```php
echo empty($order->rate) ? 'empty' : 'no empty'; // no empty
```

这一点很容易记错。

https://github.com/yiisoft/yii2/blob/master/docs/internals/core-code-style.md


如何用最简单的方法将 PDF 文件转换成图片
--------------------------------------

首先，在 Debian 上安装 `imagemagick` 包。安装后就可以在命令行使用 `convert` 命令了。下面是用 PHP 操作的过程：

```php
$pdfFile = '/tmp/a.pdf';
$pngFile = '/tmp/a.png';

exec("convert -density 300 {$pdfFile} {$pngFile}");
```

## 如何将字符串类型的 PDF 内容保存为 PDF 文件？

有的快递公司的 API 会将电子运单的 PDF 以字符串形式返回，如何在浏览器中显示呢？假设 PDF 文件保存在 `$strPdf` 变量内：

```php
header('Content-Type: application/pdf');
echo $strPdf;
```

或者：

```php
$pdf = fopen('/tmp/a.pdf, 'w');
fwrite($pdf, $strPdf);
fclose($pdf);
```

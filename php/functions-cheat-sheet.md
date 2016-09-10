# Funtions Cheat-sheet

Array Functions
============================

### Array Sorting


Name | Sorts by | Maintains key association | Desc.
-----|----------|---------------------------|--------------
asort(), arsort() | value | yes | `['Customer A' => 600.50, 'Customer B' => 700.3]` 按销售排序客户数组
ksort(), krsort() | key | yes | `['Customer A' => 600.50, 'Customer B' => 700.3]` 按**客户名称** 排序客户数组
sort(), rsort() | value | no | 适合排序非关系数组


String Functions
============================

* `mb_substr`: 截取中文字符串

  ```php
  $name = '玉不琢不成器';
  echo mb_substr($name, 2, 1); // '琢'
  ```
  
* `mb_strlen`: 获取中文字符串长度

  ```php
  $name = '玉不琢不成器';
  echo mb_strlen($name); // 6
  ```
* `http_build_query()`: 将数组转换成 query string
    
  ```php
  $data = [
      'name' => 'jim',
      'age' => 20,
      'sex' => 'male',
  ];
  echo http_build_query($data); // 'name=jim&age=20&sex=male'
  ```

* `md5()`: 返回一个字符串的 MD5 值

  
  ```php
  echo md5('drodata'); // 长度为 32 的字符串
  ```
* `md5_file()`: 返回一个文件的 MD5 值
  
  ```php
  echo md5_file('/tmp/a.jpg');
  ```

Number Functions
============================

- `abs()` 数字的绝对值
    
  ```php
  echo abs(-4.2); // 4.2
  ```

  **注意**: 该函数的值不可能是负数。比较两个浮点数是否相等用的就是该函数。
    
  ```php
  echo abs(($balance - $charge) / $charge) < 0.0001 ? 'equal' : 'not equal';
  ```

- `mt_rand(10, 20)`: generate a random integer
- 生成随机浮点数
  
  使用场景：随机生成假的产品价格。PHP 内没有内置的函数，但我们可以自己写一个：

  ```php
  function f_rand($min = 0, $max = 1, $precision = 100000) {
      if ($min > $max) {
          return false;
      }
  
      return mt_rand($min * $precision, $max * $precision) / $precision;
  }
  ```

  讲到随机数，就会想到在 jQuery 内如何生成一个随机整数。可以借助 JavaScript 原生的 `Math.random()` 函数变相实现，`Math.random()` 生成的是 0 至 1 之间的随机浮点数。假如我想生成 20 到 30 之间的整数，可以这么写：

  ```js
  var randInt = 20 + Math.floor( Math.random() * 10 );
  ```


  
Filesystem Functions
============================

* `pathinfo()`: 返回一个文件的路径信息

```php
$path_parts = pathinfo('/tmp/road.jpg');

echo $path_parts['dirname']; // '/tmp' 注意，后面没有斜杠
echo $path_parts['basename']; // 'road.jpg'
echo $path_parts['extension']; // 'jpg'
echo $path_parts['filename']; // 'road'
```

图片处理时，经常需要用该函数来获取图片的后缀命。

# Funtions Cheat-sheet

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

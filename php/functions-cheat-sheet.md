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

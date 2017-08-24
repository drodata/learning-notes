# 理解引用(Reference)

手册中相关的章节有：

- [Reference Explained][references-explained]
- [Objects and Reference][objects-and-references]
- [赋值语句提到对象赋值时按引用赋值][assignment-operator]

## Relation between Variable Name, Variable Value and Reference

PHP 中的变量名和变量值是不同的，同一个变量值可以有不同的变量名。引用的本质就是提供了一种方法：让不同的变量名称可以访问（或指向）同一个变量值。

我们可以设想 PHP 内部有一个表格，此表格只有两列：变量名和变量值。 

```php
$name = 'jack';
```

变量名是 $name, 值是 'jack'. 这行执行后，表格中会插入一行输入如下：

变量名|变量值
------|------
name | 'jack'

下面为 $name 创建一个引用：

```php
$aliasName =& $name;
```

这个语句执行后，变量值仍只有一个, 两个变量名都指向该变量值。

引用其实是 symble table aliases, 即别名。提到别名，git 命令中和 Yii2 中都由别名。

## Three basic operations

使用应用做的三种常见操作：按引用赋值、按引用传递和按引用返回。与“按引用”对应的是“按值”。

### Assign By Reference

开头已经有例子了，语法就是在等号后面紧跟一个 `&` 符号。

### Passing By Reference

这里指的是函数的参数中使用了引用，语法是在函数定义中的变量名称前面加上 `&` 符号，例如 `doSomething(&$array) {}`.

> This is done by making a local variable in a function and a variable in the calling scope referencing the same content. 

这里写一个自己看到的例子：

```php
// BaseHtml in Yii2
public static function remove(&$array, $key, $default = null)
{
    if (is_array($array) && (isset($array[$key]) || array_key_exists($key, $array))) {
        $value = $array[$key];
        unset($array[$key]);
        return $value;
    }
    return $default;
}
```

注意：`&` 仅仅出现在函数参数定义中，函数内部的变量（本例子中是 `$array`）并不加 `&`.

### Returning by Reference

这里是指函数返回的是引用，语法相比前两个复杂一下：函数名称前加 `&`, 调用函数时也加 `&`, 例如：

```php
class foo {
    public $value = 42;

    // 注意 '&'
    public function &getValue() {
        return $this->value;
    }
}

$obj = new foo;
// 注意 '&'
$myValue = &$obj->getValue();
```

## Symbol Table

手册中没有对应的概念解释，谈到这个概念的地方有：

- 手册中讲什么是引用时，讲到引用实际上是 symbol table aliases.
- PHP 函数 extract() 的作用：Import variables into the current **symbol table** from an array

从[网上][reference-turotial]找到一段定义："a structure mapping variable names onto their values" 即变量名与变量值之间的映射表。引用一节已经提到。
#Ref 

[reference-turotial]: http://code.stephenmorley.org/php/references-tutorial/
[objects-and-references]: http://php.net/manual/en/language.oop5.references.php
[assignment-operator]: http://php.net/manual/en/language.operators.assignment.php
[references-explained]: http://php.net/manual/en/language.references.php


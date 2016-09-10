# Popover

声明：以下例子都是在 Yii2 内的。

## HTML 使用

首先，在页面顶部写下面一行代码，激活页面内所有通过 HTML 标签属性设置的 popovers:

```php
// in view
$js = <<<JS
$('[data-toggle="popover"]').popover()
JS;
$this->registerJs($js);
```

常见用法：在按钮标签内部放置 `data-*` 属性来配置 popover:

```php
echo Html::button(Html::icon('star'), [
    'class' => 'btn btn-sm btn-primary',
    'data' => [
        'toggle' => 'popover',
        'title' => 'Hello',
        'content' => 'world',
        'trigger' => 'hover click',
    ],
]);
```

## JS 使用

```js
$('#my-popover-btn').popover({
    title: 'Goog',
    content: 'very good.',
    trigger: 'hover'
});
```

## Options

Name | Type | Default | Desc.
-----|------|---------|------
content | string | '' |
title | string | '' |
trigger | string | 'click' | 其它值：`hover`, `focus` (在 `<input>` 元素上 popover 会用到)
html | boolen | false | popover 内容是否支持 HTML 内容
placement | string | 'right' | 其它可选值：`auto`, `top`, `bottom`, `left`

## Methods

- `.popover('show')`
- `.popover('hide')`
- `.popover('destroy')`
- `.popover('toggle')` ?

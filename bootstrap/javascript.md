# Modal

# Tabs

在 Yii2 内使用 Tabs 很简单，主要就是配置一个 items 数组：

```php
$items = [
    [
        'label' => Html::icon('dashboard') . 'Home',
        'encode' => false,
        'active' => true,
    ],
    [
        'label' => 'Products',
        'items' => [
            [
                'label' => 'EIMS',
                'url' => 'http://b.com',
            ],
        ],
    ],
];
echo Tabs::widget([
    'items' => $items,
]);
```

这个`$items` 的配置和 Nav 中一样。除 `$items` 外其它几个常用的配置：

- boolean `renderTabContent`: 是否显示 Tab 的内容。如果用 Tabs 来实现页面导航（类似 GitHub），可以将该值设置为 false.
- string `navType`: 默认值是 `nav-tabs`, 还有一个值是 `nav-pills` (样式和 Stackoverflow 上的类似)

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


# Tooltip

Tooltip 和 Popover 相似，且更简单。

## 全局调用

`<a>` 内的 `title` 属性随处可见，如何只用一行代码就能把页面内的所有链接都改用 tooltip 显示 `title` 内的值呢？

```js
$('a:not([data-toggle])').tooltip()
```

注意我们使用了 jQuery 的 `:not()` selector 来找到所有不含 `data-toggle` 属性的链接。对 data-toggle 值已经设置的链接不使用 tooltip 的原因是：bootstrap 的 `data-toggle` 属性一次只能有一个。对一个 data-toggle 值为 'dropdown' 的具有下拉功能的链接来说，再加上 tooltip 功能将导致之前的 dropdown 功能失效。

## 长文字换行解决办法

如果 `title` 的内容过长，tooltip 就会换行显示，看起来很难看。要想确保所有内容都在一行显示，需要将 `.tooltip-inner` 的 CSS 规则覆盖为：

```css
.tooltip-inner {
    white-space: pre;
    max-width:none;
}
```

## 使用 Pjax 的 GridView 翻页后 tooltip 失效

TBD

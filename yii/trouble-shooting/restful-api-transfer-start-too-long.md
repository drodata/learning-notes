# RESTful API 请求时间过长的问题排查

起因
----------------------------------------------
打算把商品标签的二维码内容用小程序呈现。做了一个 `order-delivery-item/track` 的 API. 检测报告和检测图片内容通过 `extraFields()` 呈现。用 postman 测试该 API 在线上版本发现请求时间长达 30s. 其它还包括:

1. 仅线上版速度慢，本地测试正常；
2. 单独 `expand=analysis` 正常，只要包含 `graphics` 就慢；

排查
----------------------------------------------
起初怀疑是 `InspectionGraphics` 关系链过导致耗费时间，但 `InspectionAnalysis` 和它的结构类似，并没有问题。

随后怀疑是使用 `extraFields()` 导致的。把所有检测相关内容统一放在 `fields()` 内仍然出现慢的现象，表明问题也不在这里。

想有没有一种工具：能查看 API 请求的各个阶段耗费的时间。了解到类似 backfire.io 这样的分析工具，但这些过于复杂。

postman 上能看出时间都花费在 "Transfer Start" 这里，据了解，这个时间久大概率是自己的逻辑代码有问题。于是简化 track action 代码测试:

```php
public function actionTrack($hash)
{
    $model = OrderDeliveryItem::findOne(['hash' => $hash]);
    return $model->attributes;
}
```

上面的代码瞬间响应。这表明问题还是在 `OrderDeliveryItem::fields()` 内。排查到下面的代码时发现了问题：

```php
public function fields()
{
    $fields = parent::fields();

    return ArrayHelper::merge($fields, [
        ...
        'width' => function ($model) {
            return $model->media->getWidth();
        },
        'height' => function ($model) {
            return $model->media->getHeight();
        },
    ]);
}
```

只要去掉 `width` 和 `height`, 时间立刻缩短。而且这两个之保留任何一个，时间都是原来的一半。换句话说，这两个值每个都花费了 15s.

```php
// Media.php
public function getSize()
{
    if (!$this->isImage) {
        return null;
    }
    list($width, $height) = getimagesize($this->url);

    return [$width, $height];
}
```

直到看到这里，才意识到问题出在 `getimagesize()` 函数上。

疑问
----------------------------------------------
为啥线上运行此函数会花这么多的时间呢？

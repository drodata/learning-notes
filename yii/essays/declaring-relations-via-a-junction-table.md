# Declaring Relations via a Junction Table

在多对多关系的表格结构中，最常见莫过于通过一个关联表来实现。假设有两个表格 `product` 和 `image`, 分别存储产品数据和图片数据；每一个产品可以有多张产品图片，此时，我们可以新建一个 `product_image` 关联表来实现：

![多对多关系图 1](http://share.drodata.com/wp-content/uploads/2016/09/many2many-1.png)

声明上述关系时，有两种方式：一是借助 `via()`:

```php
class Product extends ActiveRecord
{
    public function getProductImages()
    {
        return $this->hasMany(ProductImage::className(), ['product_id' => 'id']);
    }

    public function getImages()
    {
        return $this->hasMany(Image::className(), ['id' => 'image_id'])
            ->via('productImages');
    }
}
```

二是借助 `viaTable()`:

```php
class Product extends ActiveRecord
{
    public function getImages()
    {
        return $this->hasMany(Image::className(), ['id' => 'image_id'])
            ->viaTable('product_image', ['product_id', 'id']);
    }
}
```

## 横向扩展

我们可以依葫芦画瓢，对所有含有多张图片的模型都如此声明。例如，假设有一个订单表 `order`, 每个订单都有多张包裹图片，我们可以照例新建一个 `package_image`, 来存储包裹图片：

![多对多关系图 2](http://share.drodata.com/wp-content/uploads/2016/09/many2many-2.png)

## 合并压缩

`product_image` 和 `package_image` 两个表除了第 2 列存储的外键含义不同之外，其它完全一样。将这两个表合并成一个 `map` 表，并通过 `map.type` 来区分映射的类型，`product2image` 表示产品图片的多对多映射，`order2image` 表示包裹图片多多映射。

![多对多关系图 3](http://share.drodata.com/wp-content/uploads/2016/09/many2many-3.png)

我们不再在表内显性声明外键，而是通过在 AR model 内声明这些关系。合并后的产品表图片关系如下：

```php
class Product extends ActiveRecord
{
    public function getProductMaps()
    {
        return $this->hasMany(Map::className(), ['from' => 'id'])
            ->onCondition(['type' => 'product2image']);
    }

    public function getImages()
    {
        return $this->hasMany(Image::className(), ['id' => 'to'])
            ->via('productMaps');
    }
}
```

**注意：** 使用 Map 表后，只能使用 `via()` 来声明。


参考：

- http://www.yiiframework.com/doc-2.0/guide-db-active-record.html#junction-table


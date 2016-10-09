# Migration

- http://www.yiiframework.com/doc-2.0/yii-db-migration.html

## 常用方法

- `integer()`: 如果不指明 length, 对 MySQL 来说，默认长度为 `11`;
- `unsigned()`: 无符号。注意，没有 `signed()`, 默认就区分正负；
  
  在设计数据库表格时，要**为可扩展性留有余地，不要轻易使用 unsigned integer / decimal**. 例如，假设 `price`, `quantity` 和 `charge` 分别表示一个订单订货的金额、数量和金额，`quantity` 和 `charge` 就不适合使用 unsigned 限制，因为如果将来订单表牵涉到退货环节，数量和金额都会是负数；相反，单价列无论合适都不会是负数，可以使用 unsigned 限制。
- `addPrimaryKey()` 添加主键
  
  ```php
  $this->addPrimaryKey('product2group', 'price', ['product_id', 'group']);
  ```

Methods in `yii\db\ColumnSchemaBuilder`

- `after()`: 指定新增列的位置，例如：
  
  ```php
  $this->addColumn('order', 'weight', $this->decimal(4,2)->notNull()->after('status'));
  ```

## FAQs and Howtos

### 在 up() 内使用 `Yii` 时的注意事项

Migrations 不属于任何命名空间。在文件顶部不必 `use` 其它类，直接使用 full qualified class names 即可。在下面的例子中，我们需要在 migration 时新增一个角色，所以需要使用 `Yii::$app->authManager`, 此时，**不要在顶部 `use Yii`**, 直接使用即可：

```php
use yii\db\Migration;

class m160906_053616_add_exponent_to_product extends Migration
{
    public function safeUp()
    {
        $this->addColumn('product', 'exponent', $this->decimal(4,2)->unsigned()->notNull()->defaultValue(0));

        $auth = Yii::$app->authManager;
        $legacy = $auth->createRole('legacy');
        $legacy ->description = 'xxx';
        $auth->add($legacy);
    }
}
```

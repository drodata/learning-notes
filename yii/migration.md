# Migration

FAQs and Howtos
=================

在 up() 内使用 `Yii` 时的注意事项
--------------------------------

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

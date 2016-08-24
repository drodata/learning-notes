# ActiveRecord

## `scenarios()`

有一个会计客户的表，声明的规则如下：

```php
public function rules()
{
    return [
        [['code', 'name', 'parent', 'direction', 'category',], 'required'],
        [['code', 'direction', 'category', 'parent'], 'integer'],
        [['name'], 'string', 'max' => 40],
        [['name','code'], 'unique'],
        [['assist', 'foreign_currency', 'visible'], 'safe'],
    ];
}
```

现在需要新建一个 scenario, 用来管理报销相关的科目，这些客户只需要用户输入 `name` 和 `parent` 两列的值，其余的值又程序自动生成。我们只需要重写一下 `scenarios()` 方法即可：

```php
public function scenarios()
{
    // 这里获取 SCENARIO_DEFAULT 的属性
    $default = parent::scenarios();
    $custom = [
        self::SCENARIO_EXPENSE => ['name', 'parent'],
    ];
    return yii\helpers\ArrayHelper::merge($default, $custom);
}
```

这样做的好处是，默认 scenario 不需要额外声明：

```php
$subject = new Subject;

$subject = new Subject(['scenario' => Subject::SCENARIO_EXPENSE]);
```

`scenarios()` 设置不好的话，就会出现下面的错误：

> Invalid Parameter – yii\base\InvalidParamException 
>
> Unknown scenario: default

# 创建循环付款的流程

本文简述循环付款的整个流程。

## 1 Creating Plan

包括定义、创建和激活三步。

### 1.1 定义

```php
$plan = new Plan([
    'name' => 'SAAS Month Service',
    'description' => 'Prop Account',
    'type' => 'INFINITE',
]);

$paymentDefinition = new PaymentDefinition([
    'name' => 'Pro Shop Service Plan',
    'type' => 'REGULAR',
    'frequency' => MONTH,
    'frequency_interval' => '1', // every 2 days
    'cycles' => '0',
    'amount' => new Amount([
        'currency' => 'AUD',
        'value' => 30,
    ]),
]);

$redirectUrl = 'https://example.com';
$merchantPreferences = new MerchantPreferences([
    'return_url' => Url::to(
        [
            'test/execute-agreement',
            'success' => 'true',
            'redirectUrl' => $redirectUrl 
        ], true),
    'cancel_url' => Url::to(
        [
            'test/execute-agreement',
            'success' => 'false'
        ], true),
    'auto_bill_amount' => YES,
    'initial_fail_amount_action' => CONTINUE,
    'max_fail_attempts' => '0',
]);
$plan->setPaymentDefinitions([$paymentDefinition]);
$plan->setMerchantPreferences($merchantPreferences);
```

### 1.2 创建、激活

```php
$plan->create(Yii::$app->paypal->context);

$patch = new Patch([
    'op' => 'replace',
    'path' => '/',
    'value' => new PayPalModel(['state' => 'ACTIVE']),
]);
$patchRequest = new PatchRequest();
$patchRequest->addPatch($patch);
$plan->update($patchRequest, Yii::$app->paypal->context);

return Plan::get($plan->getId(), Yii::$app->paypal->context);
```

## 2 Creating Billing Agreement

包含定义、创建和执行三步。

### 2.1 定义、创建

```php
public function actionCreateAgreement()
{
    $agreement = new Agreement([
        'name' => 'Premium Shop Service Plan',
        'description' => 'Pro Shop Service Plan',
        'start_date' => '2017-05-13T00:00:00Z',
        'plan' => new Plan([
            // 第一步中会返回 plan id.
            'id' => 'P-xxxxxxx71PB14524587N5QQ6Y',
            
        ]),
        // 支付方式是 PayPal 而非信用卡
        'payer' => new Payer(['payment_method' => 'paypal']),
    ]);
    $agreement->create($this->apiContext);
    return $this->redirect($agreement->getApprovalLink());
}
```

执行上面的 action 会将页面引导至 PayPal 付款页面，付款人需要使用自己的 PayPal 帐号登录。登录后会看到 plan 的详情，选择同意后即表示付款人和商家之间达成协议，同时页面将跳转到 `MerchantPreferences` 中定义的 return url, 在这个跳转的 url 内，我们可以执行 agreement 的操作：

### 2.2 执行

```php
public function actionExecuteAgreement($success)
{
    if (!$success) {
        return 'Failed to create payment';
    }

    $token = Yii::$app->request->get('token');

    $agreement = new Agreement();
    $result = $agreement->execute($token, $this->apiContext);

    echo '<pre>';
    print_r($result);
    echo '</pre>';
}
```

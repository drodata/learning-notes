# The PayPal PHP SDK

[PayPal PHP SDK][1] 是 PayPal RESTful API 的 PHP SDK. 这里主要记录将它整合进 Yii2 应用的流程。

## 一 快速开始

### 1.0 前提

- 一个 PayPal 账号，个人账号或商家账号均可；
- 有一个现成的 Yii2 App, 这里以 Yii2 Advanced App 模板为例；

### 1.1 获取 PayPal 开发 Client Id and Secret

登录 [PayPal Developer Dashboard][2], 在 "REST API apps" 章节 "Create App", 之后查看新建的应用，会看到 api credentials, Credentials 分 Sandbox 和 Live 两个模式，前者供测试，后者用于生产环境。我们将 Sandbox 内的 Client ID 和 Secret 两个属性的值保存下来，后面会用到这两个值。

### 1.2 安装部署

在项目根目录下执行：

```
composer require "paypal/rest-api-sdk-php:*"
```

在 `backend\components` 目录（可自行创建）下新建一个名为 `PayPal.php` 的文件, 内容如下：

```php
namespace backend\components;

use yii\base\Component;
use PayPal\Rest\ApiContext;
use PayPal\Auth\OAuthTokenCredential;

class PayPal extends Component 
{
    public $clientId;
    public $clientSecret;
    private $_apiContext;

    function init() 
    { 
        $this->_apiContext = new ApiContext(
            new OAuthTokenCredential($this->clientId, $this->clientSecret)
        );
    }

    public function getContext() 
    {
        if (!empty($this->configs)) {
            $this->_apiContext->setConfig($this->configs);
        }
        
        return $this->_apiContext;
    }
}
```

在 backend configuration file 内增加 `paypal` component:

```php
// backend/config/main.php

'components' => [
    // ...
    'paypal' => [
        'class' => 'backend\components\PayPal',
        // 使用 1.1 中保存的值填充下面两个属性
        'clientId' => '',
        'clientSecret' => '',
    ],
],
```

至此，我们已经将 SDK 和 Yii2 应用整合在一起，通过 `Yii::$app->paypal` 即可获取 api context.

> What is PayPal Api Context
> 
> ApiContext object is a context object that holds/relates to configurations that you want to pass to each request to PayPal SDK.
> 
> -- [wiki][3]

### 1.3 Sandbox

在我们 "hello world" 前，先简单了解一个 PayPal 的 Sandbox enviornment 和对应的 test accounts. 所谓沙盒环境，就是用来仿真生产环境、方便开发者测试的一个环境。开发者在测试代码时，**不必使用真实账号测试，只需在沙盒环境下测试即可**. 沙盒环境的首页地址是 https://sandbox.paypal.com, 登录后会发现，它和 PayPal 的官方网站 https://paypal.com 一模一样。我曾在这里走了弯路：环境部署好后，我的第一个念头就是——模拟一下买家向商家付款的一个过程，为此，我分别注册了个人账号和商家账号，并想办法向个人账户内充了 1 美元用来做测试。测试过程中发现 PayPal "目前不支持中国账户间的付款", 这宣告自己的念头无法实现。我这才被迫读了 [PayPal Sandbox Testing Guide][sandbox-guide], 略读后立刻发现之前的思路是错误的，PayPal 早就为开发者想好了这些东西。

#### 1.3.1 Test accounts

在 [Developer Dashboard][2] 左侧 "Sandbox" 栏目下，找到 "Accounts", 会看到两个奇怪的邮箱地址，在自己注册的邮箱地址上，分别加上了 "facilitator" 和 "buyer" 后缀，这就是 PayPal 自动为我们生成的两个测试账号. 分别模拟商家和买家。查看账户 profile 会看到，所有信息都完整，我们可以修改一下密码，然后就能用这个账号登录 [PayPal Sandbox 首页][4]了。

### 1.4 Hello World

[RESTfulAPI][rest-api] 主要有九个 resources, 初次接触难免不知从何下手，这里参照官方 Wiki 从 Vault API 下手，实现将一个假的信用卡信息存储到 PayPal Vaut 的目的。

```php
// route: text/index

public function actionIndex()
{
    $creditCard = new \PayPal\Api\CreditCard([
        'number' => '1234567890',
        'expire_month' => '11',
        'expire_year' => '2019',
        'cvv2' => '219',
        'first_name' => 'Jim',
        'last_name' => 'Li',
    ]);
    
    try {

        $creditCard->create(\Yii::$app->paypal->apiContext);
        echo $creditCard;

    } catch (\PayPal\Exception\PayPalConnectionException $ex) {

        echo $ex->getData();
    }
}
```

在浏览器中访问 `test/index` route, 将返回新建的信用卡信息，这表示我们的第一个 Hello World 成功。


## 二 进阶

[1]: https://github.com/paypal/PayPal-PHP-SDK
[2]: https://developer.paypal.com/developer/applications/ "PayPal Developer Dashboard"
[3]: https://github.com/paypal/PayPal-PHP-SDK/wiki/Adding-Configurations
[4]: https://sandbox.paypal.com/
[rest-api]: https://developer.paypal.com/docs/api/ "RESTful API"
[sandbox-guide]: https://developer.paypal.com/docs/classic/lifecycle/ug_sandbox/

# Practical Yii

## 借助 SwiftMail 和 QQ 域名邮箱发送验证邮件

我们在新用户注册时搜集用户的邮箱地址，用于将来找回密码。

### 配置 QQ 域名邮箱

1. 在 QQ 邮箱的账户设置页面，找到域名邮箱的入口。然后配置自己的域名使域名邮箱生效。
2. 新建一个域名邮箱地址，并绑定到一个 QQ 号。注意，这个 QQ 号对应的邮箱要开启 POP3 服务； 
3. 在开启 POP3 时，顺便生成授权码并记下以备后面使用。这个授权码作用相当于邮箱的登录密码；

### 配置 SwiftMail

假设我们配置的域名邮箱地址为 `admin@abc.com`, 在 Yii2 内配置 `mail` 组件如下：

```php
// Application config
'mailer' => [
    'class' => 'yii\swiftmailer\Mailer',
    'transport' => [
        'class' => 'Swift_SmtpTransport',
        'host' => 'smtp.qq.com',
        'port' => '587',
        'encryption' => 'tls',
        'username' => 'admin@abc.com',
        'password' => '', // 填写前面生成的授权码而不是邮箱密码
    ],
    'messageConfig' => [
        'charset' => 'utf-8',
        'from' => ['admin@abc.com' => 'Admin']
    ],
],
```

至此，配置完成，之后就可以直接在 Yii app 内发送邮件。

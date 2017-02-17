# Practical Yii

## 个性域名绑定

假设有一个类似微店铺的应用，用户可以注册账户并创建店铺，添加商品后即可销售产品。我们想增加一个店铺个性域名的特性：用户可以将店铺绑定到自己注册的域名上。当用户通过手机访问该域名时，直接显示该店铺的首页。同时，访问支持 HTTPS 连接。

前提假设：

- 应用的 url 是 `https://m.example.com`
- 店铺 ID 与域名间的映射已保存在数据库内，知道其中任何一个，都能通过数据库查找出另一个的值。

我们想实现如下的效果是： 有个店铺的 ID 是 `hello`, 绑定的域名是 `hello.com`, 通过下面两个 url 中任意一个都能访问该店铺：

- `https://m.example.com/hello`
- `https://m.hello.com/`

在实现此特性前，我们已经部署了一个虚拟主机，大致如下：

```bash
<VirtualHost *:80>
    ServerName m.example.com
    DocumentRoot "/path/to/app/mobile/web"
    <Directory "/path/to/app/mobile/web">
           # use mod_rewrite for pretty URL support
           RewriteEngine on
           # ...
    </Directory>
</VirtualHost>
```

### 1 添加 ServerAlias

首先，设置绑定域名的 DNS, 创建一个 `m` 的 A 记录，指向应用的 ip 地址。之后，在上面的配置中 `ServerName` 行下面追加如下一行：

```bash
ServerAlias m.hello.com
```

这表示用户访问 `m.hello.com` 时，也会指向 `/path/to/app/mobile/web` 目录。

### 2 向证书中追加 m.hello.com 域名

具体参见 [Certbot](debian/certbot-https.md) 中的笔记。

### 3 配置 Default Route

假设店铺首页的 route 是 `/site/index`, 我们需要在 `actionIndex()` 内根据请求的 url 查找到对应的店铺，然后列出该店铺下的商品。

首先判断 url 的 hostname 是不是 `example.com` 如果是，表示该店铺尚未绑定域名，使用的是内部地址，我们根据 `$_GET['shop']` 查找到店铺 id; 否则表示店铺绑定了域名，我们根据自定义域名同样在数据库中查找对应的店铺 id.

找到店铺 ID 后，一切就都好办了。


-----------------------------------------------------

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

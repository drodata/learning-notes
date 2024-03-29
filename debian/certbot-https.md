# 安装 Certbot 让站点支持 HTTPS 连接

Aliyun 免费数字证书
---------------------------------------------------------------------------
- 创建证书
- 证书申请
- 填写要绑定的域名，剩余让自动填写即可
- 下载 Apache 文件, 解压下来三个文件 (已 `static` 子域名为例)
    - `static.xxx.com_public.crt`
    - `static.xxx.com_chain.crt`
    - `static.xxx.com.key`
- 借助 FTP 把三个文件上传到服务器临时目录（`/eims/file/ftp-tmp/`）, 再移动到 Apache 下专门存储证书的目录（例如 `/etc/apache2/cert`）
- edit `sites/available/default-ssl.conf`
- `sudo service apache2 restart` 重启 Apache

全局重定向 HTTP 链接至 HTTPS 链接
---------------------------------------------------------------------------
以 i.yalongdiamond.com 为例，演示配置过程。HTTP 配置文件内容如下：

```
# in /etc/apache2/sites-available/yalong.conf

<VirtualHost *:80>
    ServerName i.yalong.com
    DocumentRoot "/home/ts/www/eims/backend/web"
    <Directory "/home/ts/www/eims/backend/web">
           # ...other settings...
    </Directory>
</VirtualHost>
```

增加如下一行：
```
    Redirect permanent / https://i.yalong.com/
```

重启 Apache 即可。

## 一 安装 Certbot

官方的[安装教程](https://certbot.eff.org/#debianjessie-apache)写得很详细。

## 二 安装证书

```bash
./path/to/certbot-auto --apache certonly
```

> 在 Debian 8.4 (Jessie) 内，命令变成了 `certbot`, 命令行变成：
> 
> ```bash
> sudo certbot --apache certonly
> ```

> ## 安装时报错
> 
> Client with the currently selected authenticator does not support any combination of challenges that will satisfy the CA
>
> 原因：certbot 版本太低。需要使用 https://certbot.eff.org/docs/install.html#certbot-auto 中介绍的办法下载最新版本的脚本来安装


上面的 `certonly` 表示只生成认证文件，而不生成 Apache2 相应的配置文件。对 Yii2 应用来说，因为有多个端，我们需要一个证书内包含多个子域名，这时后面跟着若干个 `-d <domain name>` 即可，例如：

```bash
./path/to/certbot-auto --apache certonly -d backend.abc.com -d frontend.abc.com
```

生成后认证文件存放在 `/etc/letsencrypt/live` 目录内。

### 2.1 如何向现有证书中追加/删除一个域名

假设你的应用在最开始时只有前端和后端两层，域名分别为 `backend.example.com` 和 `frontend.example.com`, 根据上面的命令生成的证书仅包含这两个域名。

现在，你的应用增加了移动端 `m.example.com`, 你想让移动端也支持 HTTPS 连接。这时就要**重新安装证书**。重新安装很简单，就是把上面的命令追加 `-d m.example.com` 后再执行一次。主要特别注意的是，证书更新完后一定要重启 Apache, 新证书才会生效。

更省事的办法是将上面的操作放在一个 shell script 内，需要变更域名时，对脚本文件稍作调整后并执行即可。下面是一个脚本文件的实例。

新建一个脚本文件内容如下：

```bash
#!/bin/bash

# File name: update-certificate.sh
certbot --apache certonly -d front.abc.com -d backend.abc.com -d m.abc.com -d m.example.com

service apache2 restart
echo 'Certificate was updated.'
```

依次执行命令：

```bash
# switch to root user
sudo -i
# make executable
chmod +x update-certificate.sh

# execute
./update-certificate.sh
```


### 2.2 FAQ 多个域名必须属于同一域名下的子域名吗

下面的安装方法可行吗

```bash
./path/to/certbot --apache certonly -d www.abc.com -d www.example.com
```

答：可以。

### 2.3 一个证书最多能包含多少个域名？

在官方的 [Rate Limits](https://letsencrypt.org/docs/rate-limits/)中提到：

> The main limit is _**Certificates per Registered Domain**_ (20 per week). A registered domain is, generally speaking, the part of the domain you purchased from your domain name registrar.

> If you have a lot of subdomains, you may want to combine them into a single certificate, **up to a limit of 100 Names per Certificate**. Combined with the above limit, that means you can issue certificates containing up to 2,000 unique subdomains per week.

也就是说，每个独立域名每周可申请 20 个证书，每个证书内可绑定 100 个子域名。

## 三、配置 Apache 以支持 SSL

`/etc/apache2/sites-available` 目录下默认有一个 `default-ssl` 文件（Apache 2.4 上此文件还有 `.conf` 后缀）。里面涉及认证的 directives 有四个：

```bash
SSLEngine on 
SSLCertificateFile          /etc/letsencrypt/live/xxx/cert.pem
SSLCertificateChainFile     /etc/letsencrypt/live/xxx/chain.pem
SSLCertificateKeyFile       /etc/letsencrypt/live/xxx/privkey.pem
```

----

注意

`SSLCertificateFile` 和 `SSLCertificateChainFile` 两个 directives 仅适用于 Apache < 2.4.8, 若 Apache >= 2.4.8, 则仅需设置 `SSLCertificateFile` 即可，其值为 `fullchain.pem` 认证文件。详细参考 [Where are my certificates?](http://letsencrypt.readthedocs.io/en/latest/using.html#where-are-my-certificates)

如果这里设置不当，将出现**同一证书在桌面端正常但在移动端提示不安全**的现象。

----

对 Yii2 应用来说，我们可以每一个端存储一个配置文件。例如

- ssl-yii-app-backend
- ssl-yii-app-frontend
- ssl-yii-app-static

这么写的好处是：我们可以通过 `a2ensite ssl-yii-app-backend` 来启用此虚拟主机。对应的命令是 `a2dissite`. 重启或禁用后，Apache 都会提示你使用 `sudo service apache2 reload` 重新加载虚拟主机。

最后，`sudo service apache2 restart` 重启 Apache.

## 四、测试 https 站点

通过前面的设置，我们指定的三个域名 `frontend.abc.com`, `backend.abc.com` 和 `static.abc.com` **均使用同一个证书**，在浏览器中输入 `https://backend.abc.com` 后会发现地址栏开头部分出现了绿色的小锁的图标，这表明 SSL 设置成功。

### 4.1 https 站点中绿色小锁图标消失的问题

这是因为站点内含有 http 请求的资源（图片、js 文件等）。https 连接内不允许有 http 请求。解决方式就是把这些 http 请求全部替换成 https 请求。

### 4.2 浏览器访问提示 "This site can’t be reached"

有时还提示：

> "This site can’t provide a secure connection"

原因排查

证书没问题，多是 Apache 配置出现问题，确认如下几项内容后重试：

- ssl module 是否已启用
- Apache 默认的 default-ssl.conf 是否通过 a2ensite 显性启用

## 五、更新证书

Let's Encrypt 免费证书的有效期是 90 天，所以应在证书到期前更新。更新分为手动和自动两种。

### 5.1 手动更新证书

```bash
./path/to/certbot-auto renew
```

## 六、参考链接

- https://www.digitalocean.com/community/tutorials/how-to-create-a-ssl-certificate-on-apache-for-ubuntu-14-04 仅借助 certbot 生成 certificate 文件，通过直接配置 Apache 实现
- Apache 日志出现 "Apache: “AuthType not set!” 500 Error" 的解决办法：http://stackoverflow.com/questions/21265191/apache-authtype-not-set-500-error

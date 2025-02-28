# SSL 证书

Aliyun 免费数字证书
---------------------------------------------------------------------------

> 阿里云免费证书可申请 20 个，单个证书有效期 90 天

- 创建证书
- 证书申请
- 填写要绑定的域名(例如 www.x.com)，剩余让自动填写即可
- 下载 Apache 文件, 解压下来三个文件 (已 `static` 子域名为例)
    - `static.xxx.com_public.crt`
    - `static.xxx.com_chain.crt`
    - `static.xxx.com.key`
- 借助 FTP 把三个文件上传到服务器临时目录（`/eims/file/ftp-tmp/`）, 再移动到 Apache 下专门存储证书的目录（例如 `/etc/apache2/cert`）
- `/etc/apache2/sites-available/default-ssl.conf` 内声明证书位置；
- `sudo systemctl reload apache2.service` 重启 Apache

腾讯云免费数字证书
---------------------------------------------------------------------------
腾讯云可以免费申请 50 个单域名证书（有效期也是 90 天），且可以自由部署在任意服务器上. 在这里申请的第一个证书是 EIMS api 站点的。

申请的过程和阿里云几乎一致，只有一处不同：Apache 密钥文件的名称和阿里云不同. 对比如下(`<domain>` 是证书对应的域名)：

名称        | 英文名称                  | 阿里云文件名          | 腾讯云文件名
------------|---------------------------|-----------------------|--------------------
证书文件    | SSLCertificateFile        | `<domain>_public.crt` | `root_bundle.crt`
证书链文件  | SSLCertificateKeyFile     | `<domain>_chain.crt`  | `<domain>.crt`
私钥文件    | SSLCertificateChainFile   | `<domain>.key`        | `<domain>.key`

在服务器上可根据需要进行重命名。

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

变更
--------------------------------------------------------------------------
- 2024-12-30 阿里云将免费证书有效期缩短为 90 天

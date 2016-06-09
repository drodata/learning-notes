# Errors and Workaround

## Failed to decode zlib stream

按照官方安装教程执行第三步，执行

```bash
php composer-setup.php 
```

时，出现如下提示：

> All settings correct for using Composer
> Downloading 1.1.2...
> PHP Fatal error:  Uncaught exception 'RuntimeException' with message '**Failed to decode zlib stream**' in /home/ts/composer-setup.php:783

https://github.com/composer/composer/issues/4121#issuecomment-109977174 上已讲过，执行 `composer clear-cache` 即可。

## [ReflectionException]: Class Fxp\Composer\AssetPlugin\Repository\NpmRepository does not exist

原因：`fxp/composer-asset-plugin` 升级造成的。

解决（注意 `--no-plugins` option）：

    composer global require fxp/composer-asset-plugin --no-plugins

## [Composer\Downloader\TransportException]                                     

> The "https://getcomposer.org/composer.phar" file could not be downloaded: Failed to enable crypto failed to open stream: operation failed 

使用 shadowsocks 是会出现此信息，使用普通连接 OK.

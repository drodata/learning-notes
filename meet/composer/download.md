# Download and Install

## Download

参考链接：https://getcomposer.org/download/

之前从未安装过，从 https://github.com/composer/composer/releases 下载最新版，下载的文件为 `composer.phar`.

为了在任何地方都能执行该命令，我们执行 

```bash
sudo mv composer.phar /usr/local/bin/composer
chmod +x /usr/local/bin/composer
```

之后，即可直接使用 `composer` 命令了。

## Install The Composer Asset Plugin

```bash
composer global require "fxp/composer-asset-plugin:dev-master"
```

这是一个 composer 插件，作用是可以通过 Composer 管理 npm package. npm 是 JavaScript 下的 package manager. 由于是全局安装（`global` option），此命令仅需在首次下载 Composer 后执行一次。

## Usage

我们以安装 Yii2 advanced template 为例，来演示如何使用 Composer 下载软件：

```bash
composer create-project --prefer-dist yiisoft/yii2-app-advanced yii-application
```


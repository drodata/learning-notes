# 如何在最新版的系统上部署就版本 PHP 开发环境

场景
--------------------------------------------------------------------------
服务器上的 PHP 版本仍旧是 7.1, 而且某些 composer extension (easywechat) 在 8.2 本地环境下会报错。我又不想为此更新 composer extension. 所以就需要安装和服务器端版本一致的 PHP 版本。

演示如何在最新的系统(当前 Debian 12)上安装旧版本的 PHP. 以当前最新的系统 (Debian 12 bookworm) 为例，默认版本是 PHP 8.2, 下面演示如何安装 PHP 7.1, 其它版本类似。

步骤
--------------------------------------------------------------------------

### Step 1: 添加第三方仓库

```
sudo apt install -y apt-transport-https lsb-release ca-certificates wget
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
```

1. 安装必要的包，默认情况下，这些包都已内置安装；
2. 下载一个类似可信任的证书；
3. 在 source.list.d 目录内添加一个源；

### Step 2: 安装

```
sudo apt update
sudo apt install php7.1 php7.1-mysql php7.1-gd
```

通过更新，就能找到旧版本的 PHP. 第二行安装我们需要的两个包。

- `php7.1-mysql`: 确保数据库正常；
- `php7.1-gd`: 确保处理图片功能正常；

### Step 3: 将目标版本的 PHP 设为默认
安装完使用 `php -v` 显示的版本仍然是 8.2, 但是 `/usr/bin/` 内已经有了 PHP7.1. 下面我们把默认 php 版本设置成 7.1

```
$ sudo update-alternatives --set php /usr/bin/php7.1
```

再次验证，成功：

```
$ php -v

PHP 7.1.33-56+0~20230904.88+debian12~1.gbp4cb5d3 (cli) (built: Sep  4 2023 08:15:46) ( NTS )
Copyright (c) 1997-2018 The PHP Group
```

### Step 4: 调整 Apacha 相关模块

```
$ sudo a2dismod php8.2
$ sudo a2enmod php7.1
$ sudo systemctl restart apache2
```

打开 localhost, 本地开发环境终于正常。

总结
--------------------------------------------------------------------------
要想成功完成 PHP 版本的切换，应满足以下条件：

1. 使用 `php -v` 显示的必须是自己要用的版本. 这里曾有个误解，以为只要 Apache 内部 PHP 模块替换就够了，Cli 不用管。这种想法是错误的；
2. Apache 内 PHP 模块启用；

- 参考：[How To Install PHP (8.2, 8.1 or 7.4) on Debian 12][ref]

[ref]: https://tecadmin.net/how-to-install-php-on-debian-12/

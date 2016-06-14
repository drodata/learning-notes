Installing Locally
========================

本文简单介绍在本地机器上（Debian 8）安装 gitbook 的步骤。

1. 安装 NodeJS 和 npm
--------------------------

npm 是 Node Package Manager 的简称。它是一个 Javascript 包管理器，类似 PHP 下的 Composer.

Debian 自带的 `nodejs` 版本过低（`0.10.29`）,我们用下面的方式安装最新版的 NodeJS:

```bash
# 以 root 登录
su root

# now, the login user is root
curl -sL https://deb.nodesource.com/setup_4.x | bash -
apt-get install nodejs
```

该 `nodejs` package 已内置 `npm` package, 无需额外安装。安装完毕后，查看版本：

```bash
$ nodejs -v
v4.4.5

$ npm -v
2.15.5
```

2. 使用 npm 安装 gitbook 客户端
--------------------------

```bash
$ sudo npm install gitbook-cli -g
```

`-g` 中的 'g' 代表 global, 全局安装的意思。

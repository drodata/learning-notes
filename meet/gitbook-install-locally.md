Installing Locally
========================

本文简单介绍在本地机器上（Debian 8）安装 gitbook 的步骤。

1. 安装 NodeJS 和 npm
--------------------------

npm 是 Node Package Manager 的简称。它是一个 Javascript 包管理器，类似 PHP 下的 Composer.

```bash
sudo apt-get install nodejs nodejs-legacy npm
```

安装 `nodejs-legacy` package 是为了防止出现

> /usr/bin/env: node: No such file or directory

错误。

2. 使用 npm 安装 gitbook 客户端
--------------------------

```bash
$ sudo npm install gitbook-cli -g
```

`-g` 中的 'g' 代表 global, 全局安装的意思。

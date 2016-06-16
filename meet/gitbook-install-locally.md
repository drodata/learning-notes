Installing Locally
========================

本文简单介绍在本地机器上（Debian 8）安装 gitbook 的步骤。

1. 安装 NodeJS 和 NPM
--------------------------

`gitbook` 本质上是一个 NPM Package, 所以先要安装 Node.js 和 NPM. 参见[安装 Node.js](/meet/nodejs/install.md).

2. 使用 npm 安装 gitbook 客户端
--------------------------

```bash
$ sudo npm install gitbook-cli -g
```

`-g` 中的 'g' 代表 global, 全局安装的意思。加上 `sudo` 是因为我们安装 NPM 时是以 root 身份进行的。局部安装无需加 `sudo`.

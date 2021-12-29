# Installing Node.js and NPM

npm 是 Node Package Manager 的简称。它是一个 Javascript 包管理器，类似 PHP 下的 Composer.

## Installing on Debian

参考[官方的安装说明][install-instruction]下载最新版。

```bash
# Using Debian, as root
# curl -fsSL https://deb.nodesource.com/setup_17.x | bash -

$ sudo apt-get install -y nodejs
```

检查版本：

```bash
$ node -v
v17.3.0

$ npm -v
8.3.0
```

## Installing on Mac

Mac 下，仅需下面一条命令即可安装 Node.js 和 NPM:

```bash
$ brew install node
```

[install-instruction]: https://github.com/nodesource/distributions#installation-instructions

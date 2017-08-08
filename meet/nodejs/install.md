# Installing Node.js and NPM

npm 是 Node Package Manager 的简称。它是一个 Javascript 包管理器，类似 PHP 下的 Composer.

## Installing on Debian

参考[官方的安装说明][install-instruction]下载最新版。

```bash
# 就是一个 shell script
wget https://deb.nodesource.com/setup_8.x

# 赋予可执行权限
chmod +w setup_8.x

# 以 root 身份运行 script
sudo ./setup_8.x

# 安装
apt-get install nodejs
```

检查版本：

```bash
$ nodejs -v
v8.2.1

$ npm -v
5.3.0
```

## Installing on Mac

Mac 下，仅需下面一条命令即可安装 Node.js 和 NPM:

```bash
$ brew install node
```

[install-instruction]: https://github.com/nodesource/distributions#installation-instructions

# Installing Node.js and NPM

npm 是 Node Package Manager 的简称。它是一个 Javascript 包管理器，类似 PHP 下的 Composer.

## Installing on Debian

Debian 自带的 `nodejs` 版本过低（`0.10.29`）,我们用下面的方式安装最新版的 NodeJS (ref. https://github.com/nodesource/distributions):

```bash
# 以 root 登录
su root

# now, the login user is root
curl -sL https://deb.nodesource.com/setup_4.x | bash -
apt-get install nodejs
```

### `curl` Problem

上面的的 `curl` 命令有时可能会没有任何反应。

```
# curl -sL https://deb.nodesource.com/setup_4.x | bash -
# 
```

经过验证，有可能是 `curl` package 的问题。所幸，还可以使用 `wget` 安装。执行下面命令前，先 `apt-get purge curl`, 因为，`setup_4.x` shell script 内含有尝试使用 `curl` 安装的语句。

```bash
# wget -qO- https://deb.nodesource.com/setup_4.x | bash -
```

该 `nodejs` package 已内置 `npm` package, 无需额外安装。安装完毕后，查看版本：

```bash
$ nodejs -v
v4.4.5

$ npm -v
2.15.5
```

## Installing on Mac

Mac 下，仅需下面一条命令即可安装 Node.js 和 NPM:

```bash
$ brew install node
```

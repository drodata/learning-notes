Installing The Command Line Interface gitbook-cli
========================

[`gitbook-cli`][gitbook-cli] 是 GitBook 的命令行接口。

`gitbook` 本质上是一个 NPM Package, 所以先要[安装 Node.js 和 NPM](/meet/nodejs/install.md).

安装很简单：

```bash
$ sudo npm install gitbook-cli -g
```

`-g` 中的 'g' 代表 global, 全局安装的意思。加上 `sudo` 是因为我们安装 NPM 时是以 root 身份进行的。局部安装无需加 `sudo`.

命令行使用说明参考 `gitbook help`.

[gitbook-cli]: https://www.npmjs.com/package/gitbook-cli

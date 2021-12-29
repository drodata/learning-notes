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

```
ts@TS:~/www/learning-notes$ sudo npm install gitbook-cli -g

changed 46 packages, and audited 579 packages in 4s

45 vulnerabilities (2 low, 21 moderate, 14 high, 8 critical)

To address issues that do not require attention, run:
  npm audit fix

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
```

[gitbook-cli]: https://www.npmjs.com/package/gitbook-cli

# Errors

## `npm install`

### `EPEERINVALID`

> error code `EPEERINVALID`
>
> error peerinvalid 
>
> The package vue-hot-reload-api@2.0.1 does not satisfy its siblings' peerDependencies requirements! vue-loader@8.5.2 **wants vue-hot-reload-api@^1.2.0**

简单说，就是 `vue-loader@8.5.2` package 要求 `vue-hot-reload-api` 的版本号是 `^1.2.0`, 而准备安装的版本号是 `2.0.1`.

解决：按照要求，安装指定版本的 package 即可：

```bash
npm install vue-loader vue-hot-reload-api@^1.2.0
```

换句话说，使用 `npm install <package>` 安装 package 时，`<pcakage>` 后面可以指定版本号，格式是: `@<version-number>`.

最后安装的实际版本号是 `^1.3.2`: 

```
  "devDependencies": {
    ...
    "vue-hot-reload-api": "^1.3.2",
    "vue-loader": "^8.5.2",
  },
```

这是因为我们在版本号前面使用了 Caret range `^` 的原因。关于 `^` 的含义参见 [NPM Semver](/meet/npm/node-semver.md) 相关基础。

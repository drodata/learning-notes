Plugin
=============

插件的安装很简单，只需新建 `book.json` 文件，并写入以下内容即可：

```
{
    "plugins": ["plugin-name-1", "plugin-name-2"]
}
```

在本地使用插件（`gitbook serve`）需要使用 `gitbook install` 命令安装。插件全部安装在 `node_modules` 子目录下，类似 Composer 中的 `vendor` 目录。

所有插件在 gitbook.com 上都已预装。因此，push 代码前需要把 `node_modules` 目录忽略掉：

```
# .gitignore

...
node_modules/
```

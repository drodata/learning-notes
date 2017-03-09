# Create a book

假设我想写一个关于 Composer 的主题，首先可以在 `SUMMARY.md` 内添加提纲：

```
* [Composer](meet/composer/README.md)
    * [Download and Install](meet/composer/download.md)
    * [Errors and Workaround](meet/composer/errors-and-workaround.md)
```

然后执行 `gitbook init`, 即可自动创建提纲中的文件和目录，这样省得我们手动创建文件，非常方便。

## Linking `.md` files in pages

`SUMMARY.md` 内的提纲其实就是链接。gitbook 会自动将 md 文件转换成对应的 HTML 页面。注意，自动生成的 markdown 文件链接是相对路径（`meet/composer/download.md`）, 这是因为 `SUMMARY.md` 在根目录下。如果在其它页面内设置链接时，就要使用绝对路径了（前面加上 `/` 即可：`/meet/composer/download.md`）。范例：[Composer 的安装方法](/meet/composer/download.md).

> :warning:
>
> 要想让链接在 Gitbook 发布页面上正常显示，该 .md 文件**必须出现在 `SUMMARY.md` 文件内** (git book 会将 `SUMMARY.md` 内列出的所有 md 文件都转换成 HTML 页面). 

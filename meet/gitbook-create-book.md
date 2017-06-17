# Initialization

假设你刚从 GitHub 上创建了一个 repository, 里面只有一个 README.md 文件。在正式开始编写具体内容前，需要做一些初始化的工作，包括：

- 新增 .gitignore 文件
  
  由于我们可能使用 `gitbook serve` 在本地预览，此命令会在 repository 根目录下创建一个 `_book` 目录，目录下存储的是静态 HTML 文件，我们需要将此目录忽略掉。
  
  ```
  _book/
  npm-debug.log
  node_modules/
  ```

- 新建 `img` 目录用来存放图片信息，这样就可以在任何文档内通过 `![Alt text](/img/xxx.png)` 使用图片了；

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

# Custom Domain

所有书本在 GitBook 服务器上的 `http://{author}.gitbooks.io/{book}/` 内存储，书本的内容可通过二级域名 `http://{author}.gitbooks.io/{book}/content/` 访问. 如果你有自己的域名，使用自己的域名是个不错的选择。仅需两步即可完成：

1. 在域名的 DNS 管理页面，新增一个类型为 `CNAME`, 主机为 `manual`, 指向 `www.gitbooks.io` 的记录。
2. 在书本的 SETTINGS 页面，点击左侧 Domains 选项，属于域名 `manual.{your-domain}`


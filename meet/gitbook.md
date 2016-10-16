# GitBook

## Using GitBook in GitHub Repository

在 repository 根目录下:

1. 新建 `book.json` 文件，内容如下：

    ```js
    {
       "root": "./docs"
    }
    ```
2. 新建 `docs` 目录； 

在 `docs/` 下新建 `SUMMARY.md` 文件，里面写入章节结构，范例如下：

```md
# Summary

* [Part I](part1/README.md)
    * [Writing is nice](part1/writing.md)
    * [GitBook is nice](part1/gitbook.md)
* [Part II](part2/README.md)
    * [We love feedback](part2/feedback_please.md)
    * [Better tools for authors](part2/better_tools.md)
```

执行 `gitbook init` 新建对应的 markdown 文件。

Ref.: https://toolchain.gitbook.com/structure.html

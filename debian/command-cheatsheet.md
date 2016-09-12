# 常用命令

## Glance

- 查看一个目录的容量：`du -sh ./ftpupload`

## `tree`

list contents of directories in a tree-like format.

`tree` 命令可递归遍历一个目录，并将该目录下的文件结构以文本形式漂亮地输出。例如：

```bash
$ cd a-directory/
$ tree
```

输出：

```
.
├── components
│   ├── AppFooter.vue
│   ├── VuxTab.vue
│   └── Vux.vue
├── index.html
├── main.js
├── package.json
└── webpack.config.js

1 directory, 12 files
```

### Advanced Options

#### `-P`, `-I`

这两个参数后面都需跟一个正则表达式 `<pattern>`. 前者表示 "include", 仅显示符合 pattern 的内容；后者表示 "exclude", 不显示匹配 pattern 的内容。

实例一：

假设有下面一个目录结构：

```
├── directory-a
├── directory-b
├── .directory-c
├── file-a 
└── file-b
```

`directory-b` 和 `.directory-c` 目录下含有大量我们不关心的文件（例如 `.git`）。如果直接使用 `tree` ， 输出内容会很长，且含有大量我们不关心的文件。现在我想把这两个目录“屏蔽”掉，参数如下：

```bash
tree -I "directory-b|.directory-c"
```

注意：

- 一次匹配多个 patterns 使用的是管道符号 `|`, 注意，不要乱加空格。例如，上面的 pattern 如果写成 "directory-b | .directory-c", 结果完全不同；
- pattern 中如果是目录，后面不要加 `/`;

合法的 wildcard operators are:

- `*`: any zero or more characters;
- `?`: any **single** character
- `[...]`: any single character listed between brackets. e.g. `[123]`, `[1-9]`
- `[^...]`: any single character **not listed in** brackets
- `|`: separatoers alternate patterns;

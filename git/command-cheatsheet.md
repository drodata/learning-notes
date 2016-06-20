# Commands Cheat-sheet

## 标签操作

类别    | 命令实例 | 说明
--------|----------|------
新建    | `git tag v0.2.2-lw` | 新建一个轻量标签
        | `git tag -a v0.2.2 -m 'version 0.2.2'` | 新建一个带注释的标签
        | `git tag -a v0.0.1 9fceb02 -m 'initial version'` | 给之前的某个 commit 打标签

列表    | `git tag` | 列出 resposity 中所有tag
        | `git tag -l 'v0.2.2*`| 筛选出以`v0.2.2`开头的标签. `-l` 后面跟正则 `<pattern>`
        | `git show v0.2.2` | 查看某个标签的 Commit 信息

Push    | `git push origin v0.2.2` | 分享一个标签至远端
        | `git push origin --tags` | 一次性分享多个标签至远端

删除    | `git tag -d 0.4.1` | 删除标签 0.4.1

# Intergrate with GitHub

## 建立权限

在 GitBook 的设置页面内的 GitHub Panel 中，列出了 GitBook 对 GitHub 的操作权限，我们选择 "With access to public repositories"

## 在 GitBook 中新建一本书

假设我们新建一本名为 "Learning Notes" 的书。

3. 在 GitHub 中新建一个 repository 作为 Hosting
----------------------------

假设地址为：`git@github.com:drodata/learning-notes.git`

4. 将 GitBook 中的书和 GitHub repository 建立连接
----------------------------

在书的 Setting - GitHub 页面，输入 GitHub repository 地址，格式为 `<github-user-name>/<repository-name>`. 这里，我们输入 `drodata/learning-notes.git`. 最后，点击保存。

5. Setting up webhooks
----------------------------

第 4 步操作完成后，页面下方会新增一个 "Integration" 的 panel, 点击 "Add webhooks" 完成绑定。点击 "Check webhooks" 检查操作是否成功。成功绑定的页面如下：

![webhooks](http://share.drodata.com/wp-content/uploads/2016/06/webhook.png)

这个”钩子“建立后，我们每次向 GitHub repository 进行 push 操作时，都会触发一个事件，告知 GitBook, 第一时间同步 commits.

6. Last step
----------------------------

现在，GitBook 已经和 GitHub 上的 repository 建立连接。我们就可以像普通 reposotory 那样进行 commit 了。

`gitbook` 提供了一个本地预览页面的命令：`gitbook serve`. 它会生成一个本地的静态页面（`_book` 目录）, 通过 `http://localhost:4000` 可以访问。`_book` 目录仅用于本地预览，我们需要在 `.gitigore` 内将该目录添加进入：

```
_book/
```

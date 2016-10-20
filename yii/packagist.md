# Packagist

- [Creating your own Application structure](http://www.yiiframework.com/doc-2.0/guide-tutorial-start-from-scratch.html)

## 在 Packagit 上发布自己的第一个 Project Template

1. Modify the Files

   修改 `composer.json` 文件内容。

2. push 到 GitHub 上
3. 登录 Packagist, submit package. 直接输入项目在 GitHub 上的地址即可；
4. 设置 Auto-Update
   在项目的 GitHub 主页上，依次点击 "Settings" - "Integrations & services", on "Add service", select "Packagist", 输入三个信息：
    - User: `drodata`
    - Token: 在 Packagist Profile 页可找到
    - Domain: `https://packagist.org/`
5. 使用
   
   ```bash
   composer create-project --prefer-dist --stability=dev drodata/yii2-app-template my-next-project
   ```

   相比使用 git clone 命令，`composer create-project` 不但会下载 package 本身，**还会将 package 所依赖的所有 packages 一并下载**, 作用相当于 `git clone` + `composer install`

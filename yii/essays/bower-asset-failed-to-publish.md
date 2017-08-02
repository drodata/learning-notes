# F

在安装 Yii2 项目时，执行 `composer install` 曾不止一次遇到过如下错误提示：

> Exception 'yii\base\InvalidParamException' with message '**The file or directory to be published does not exist: `/path/to/my-project/vendor/bower/jquery/dist'`**

`/path/to/my-project/` 是项目的根目录，会因人而异。查看 `vendor` 目录，发现的确没有 `bower` 目录，有一个 `bower-asset` 目录。以前的解决办法是手动将 `bower-asset` 重命名为 `bower`. 这种办法显然没有找到根源，这一次恰巧又碰到，索性尝试彻底解决。

首先这个异常是在如下位置抛出的：

```php
//In yii\web\AssetManager.php

public function publish($path, $options = [])
{
    $path = Yii::getAlias($path);
    if (isset($this->_published[$path])) {
        return $this->_published[$path];
    }
    if (!is_string($path) || ($src = realpath($path)) === false) {
        throw new InvalidParamException("The file or directory to be published does not exist: $path");
    }
    if (is_file($src)) {
        return $this->_published[$path] = $this->publishFile($src);
    }
    return $this->_published[$path] = $this->publishDirectory($src, $options);
}
```

这也印证了问题出在路径错误上。以 `/path/to/my-project/vendor/bower/jquery/dist` 为例，`/path/to/my-project/vendor/bower` 由预定义的 alias `@bower` 解析（[出处][bower-alias]）。因此，问题转化为：**vendor 目录下存放 bower library 的根目录为什么不是 `bower` 而是 `bower-asset`？**

所有 NPM/bower library 都是通过 [`fxp/composer-asset-plugin`][fxp-composer-asset-plugin] 安装的，安装文档中讲到了[为什么目录名称是 'bower-asset'][define-a-custom-directory-for-the-assets-installation].

> By default, the plugin will install all the assets in the directory `vendors/{asset-type}-asset` and packages will be installed in each folder with their asset name.

这就是 `bower-asset` 目录的出处。阅读文档后发现另一个问题，在自己的项目内，的确也自定义了安装目录，但是和官方文档中的不太一样。自己项目中 `composer.json` 的相关配置为：

```
"extra": {
    "asset-installer-paths": {
        "npm-asset-library": "vendor/npm",
        "bower-asset-library": "vendor/bower"
    }
}
```

官方文档中自定义配置范例为：

```
{
    "config": {
        "fxp-asset": {
            "installer-paths": {
                "npm-asset-library": "vendor/npm",
                "bower-asset-library": "vendor/bower"
            }
        }
    }
}
```

这是因为 `fxp/composer-asset-plugin` 在 [v1.3.0][v1.3.0] 中弃用了一批参数，其中包括将 `extra.asset-installer-paths` 改为 `config.fxp-asset.installer-paths`. 但是 relase note 中讲得很清楚：此处改进是向后兼容的，换句话说，之前的旧配置也管用。一个合理的推测是，composer.json 配置内容和 `fxp/composer-asset-plugin` 版本不匹配的话，会导致配置文件中的自定义安装目录未能生效，进而导致使用默认的目录进行安装。

我先后在两个项目上实验成功，测试环境为：

- Composer: 1.4.2
- fxp/composer-asset-plugin: 1.3.1
- Yii2: 2.0.12

具体步骤为：

1. 执行 `composer self-update` 升级 Composer 至最新；
2. 执行 `composer global update fxp/composer-asset-plugin` 升级至最新版；
3. 更新项目 composer.json 中自定义 assets 的安装目录：

   ```
   {
       "config": {
           "fxp-asset": {
               "installer-paths": {
                   "npm-asset-library": "vendor/npm",
                   "bower-asset-library": "vendor/bower"
               }
           }
       }
   }
   ```

4. 清空 `vendor` 目录
5. `composer update` (注意是 update 非 install, 因为前面的 composer.json 内容已改变，需要执行 update 更新 composer.lock 文件)

按照上面的步骤操作后，`vendor` 目录下成功出现 `bower` 子目录，之前的 `bower-asset` 消失了。



[chartjs]: https://github.com/chartjs/Chart.js
[bower-alias]: https://github.com/yiisoft/yii2/blob/2.0.12/framework/base/Application.php#L461
[fxp-composer-asset-plugin]: https://github.com/fxpio/composer-asset-plugin
[define-a-custom-directory-for-the-assets-installation]: https://github.com/fxpio/composer-asset-plugin/blob/master/Resources/doc/index.md#define-a-custom-directory-for-the-assets-installation
[v1.3.0]: https://github.com/fxpio/composer-asset-plugin/releases/tag/v1.3.0

# FAQs

## 常用命令

### 如何获取当前已安装的 packages 信息（及版本号）？ 

```
composer [global] show
```

不带 `global` 显示的是当前项目下安装的包，带 `global` 显示全局安装的包。显示内容大致如下：

```
2amigos/yii2-file-upload-widget    1.0.4              Blueimp file upload widget for the Yii framework
2amigos/yii2-gallery-widget        1.0.2              Blueimp gallery widget for the Yii framework
alexgx/yii2-phpexcel               dev-master 5078b80 PhpExcel component extension for Yii2.
```

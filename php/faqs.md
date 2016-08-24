# PHP FAQs

## 如何用最简单的方法将 PDF 文件转换成图片

首先，在 Debian 上安装 `imagemagick` 包。安装后就可以在命令行使用 `convert` 命令了。下面是用 PHP 操作的过程：

```php
$pdfFile = '/tmp/a.pdf';
$pngFile = '/tmp/a.png';

exec("convert -density 300 {$pdfFile} {$pngFile}");
```

## 如何将字符串类型的 PDF 内容保存为 PDF 文件？

有的快递公司的 API 会将电子运单的 PDF 以字符串形式返回，如何在浏览器中显示呢？假设 PDF 文件保存在 `$strPdf` 变量内：

```php
header('Content-Type: application/pdf');
echo $strPdf;
```

或者：

```php
$pdf = fopen('/tmp/a.pdf, 'w');
fwrite($pdf, $strPdf);
fclose($pdf);
```

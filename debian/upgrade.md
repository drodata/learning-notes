# 升级记录

步骤
--------------------------------------------------------------------------

### Update sources
编辑 `/etc/apt/sources.list`, 将版本关键词替换成最新。以本次为例，将所有的 `bullseye` 替换成 `bookworm`;

### APT Update

```
sudo apt update  > 升级本地 packages list.
```
### APT Upgrade
分成两步：

首先执行

```
sudo apt upgrade --without-new-pkgs
```
升级但不删除 package

然后，执行

```
sudo apt full-upgrade
```
这里是升级的真正环节，软件就是在这一步内完成版本升级。
   
> 注意：这一步不要离开屏幕，因为有很多交互需要处理，否则会导致升级不完整

### Reboot and Verify

Reboot:

```
sudo reboot
```

Verify:

```
uname -mrs 
Linux 6.1.0-13-amd64 x86_64

lsb_release -a
No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 12 (bookworm)
Release:	12
Codename:	bookworm
```

### Remove useless packages
删除过期的包（可选项）

```
sudo apt --purge autoremove
```

Logs
--------------------------------------------------------------------------

### Debian 11 to 12 (2023-11-30)

升级后 locahost 打不开。联想到升级中对话框选择保留了旧的配置文件，怀疑是配置文件错误的原因。后来找到原因：升级中 PHP 由 7.1 变成了 8.2, Apache 对应的模块没有设置妥当。

```
# 禁用旧的模块
sudo a2dismod php7.1

# 启用当前版本的模块
sudo a2enmod php8.2

sudo systemctl restart apache2
```

重启后显示出来，但是提示 could not find driver, 那是 MySQL 没有设置好的原因：

```
sudo apt install php8.2-mysql

sudo systemctl restart apache2
```

剩下的都是 PHP 比较简单的 warning 或 notice, 例如：

```
PHP Warning – yii\base\ErrorException
Undefined array key "class"
```

简单调整一下即可。

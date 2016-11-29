# ECS Initialization

在阿里云购买 ECS 云主机后，上面只有一个操作系统（我选择的是 Debian），接下来，我们需要安装常用的工具包，搭建运行环境。

我们通过 ssh 登录主机：

1. 新建一般用户 `ts`，并设置密码
   
   ```bash
   useradd -d /home/chen -s /bin/bash -m chen
   passwd chen
   ```
2. 让普通用户 `ts` 具有 sudo 权限
	1. 安装 `sudo` package
	   
	   ```bash
	   # apt-get install sudo
	   ```
	2. 临时修改 `/etc/sudoers` 文件权限，使得 root 可以直接使用 vim 编辑：

	   ```bash
	   # chmod +w /etc/sudoers
	   ```
	3. 编辑 `/etc/sudoers`, 在里面找到如下一行：

	   ```bash
	   root    ALL=(ALL:ALL) ALL
	   ```

	   在后面追加一行：

	   ```bash
	   ts      ALL=(ALL) NOPASSWD: ALL
	   ```

	   保存，退出。以后 `ts` 就能使用 `sudo` 了。

## 3 设置 ts .vimrc, .bashrc 文件

## 4 Installing PHP, MySQL and Apache

```bash
sudo apt-get update
sudo apt-get install php5 mysql-server mysql-client php5-mysql php5-curl php5-gd
```

安装过程中需要有一个额外的交互——设置 MySQL 密码。

### 4.1 配置 Packages

- PHP: 更改 `error_reporting` directive, 不要报告 `E_NOTICE` 错误；
- Apache: 记得启用 `Rewrite` engine: `sudo a2enmod rewrite`. 如果站点使用 HTTPS 安全连接，还需启用 `ssl` mod.

## 5 测试 Apache

我需要把默认的 Document Root 指向 /home/ts/www/ 目录。
	   
```bash
vi /etc/apache2/sites-aviable/000-default
# 将 DocumentRoot 的值由 /var/www 改成 /home/ts/www, 保存并退出
# 重启 Apache
sudo /etc/init.d/apache restart
# 在 /home/ts/www/ 下新建 index.php, 里面调用 phpinfo()
```

这时访问 ip, 提示 403 错误，查找后发现，还需要再修改一处：

```bash
vi /etc/apache/apache2.conf
```

找到：

```bash
<Directory /var/www/>
		Options Indexes FollowSymLinks
 	AllowOverride None
 	Require all granted
</Directory>
```

将 `/var/www/` 替换为 `/home/ts/www/`. 再次重启 Apache, 这次访问首页，成功出现 phpinfo 内容.

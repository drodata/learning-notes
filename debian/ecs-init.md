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
3. 设置 ts .vimrc, .bashrc 文件
3. 安装常用软件包（以下 packages 均使用 `sudo apt-get install <package-name>` 安装）
	1. 安装 PHP, Apache php5, php5-curl, php5-gd. 安装 php5 时，package `apache2` 作为其依赖包也一并安装；
	2. 测试。我需要把默认的 Document Root 指向 /home/ts/www/ 目录。
	   
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
	3. 安装 MySQL 
	   
	   ```bash
	   sudo apt-get install mysql-server mysql-client php5-mysql
	   ```

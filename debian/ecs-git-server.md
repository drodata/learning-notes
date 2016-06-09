# Setup Git Server

本文介绍如何在阿里云 ECS 云服务器上搭建 Git 服务器，实现在自己的服务器上托管代码的目标。

## 1 安装 git

新购买的 ECS 上只有一个操作系统，没有软件，我们需要先安装`git`软件：

```
sudo apt-get install git
```

## 2 添加专门执行 git 操作的用户 `git`

```bash
sudo adduser git
```

## 3 创建`authorized_keys`文件，添加开发者 SSH Public Keys

```bash
# switch to user git we just created
su git
# go to git's home directory
cd
# 新建 .ssh 目录，更改权限（默认的权限是755）
mkdir .ssh && chmod 700 .ssh
# 新建用于存储开发者 SSH Public Keys 的文件，并设置权限
touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```

下面，将自己电脑上的 SSH Public Keys 追加到`authorized_keys`文件内。公钥所在位置为`~/.ssh/id_rsa.pub`。添加后自己电脑就拥有了 git 的读写权限，而且再使用

	ssh git@server

登录 ECS 时可以不用再输入密码了。

## 4 在服务器端新建 Reposity

这一步相当于在 GitHub 上新建一个 repository.

目前我托管的 Repository 都是 Web Application, 除了用 git 进行版本控制外，还需要可以直接访问。执行如下命令：

```bash
# 1 切换到存放 Repository 的目录下，这里我们存放在 /var/www 下，
#   即 Apache 的 root
cd /var/www

# 2 新建 Web Application 目录名称，进入该目录下
mkdir eo
cd eo

# 3 初始化 git
git init

# 4 为 no-bare 库添加设置
git config receive.denyCurrentBranch ignore

# 5 添加 hook
cd .git/hooks
touch post-receive && chmod +x post-receive
```

编辑上面新建的文件`post-receive`,添加如下内容：

```bash
!/bin/sh
cd .. 
env -i git reset --hard
```
至此，服务器端的设置告一段落。

## 5 在本地新建git库，开始与远端交流

### 托管一个全新的项目

```bash
# 在刚才添加SSH公钥的电脑上执行
cd myproject
git init
git add .
git commit -m 'initial commit'
git remote add origin git@server:/var/www/blog
git push origin master
```
这样就将 Commit push 到 ECS 上。

### 托管 GitHub 上已有项目

这时只需再添加一个 remote 即可。以`eo/`为例， `eo` 已有一个名为 `origin` 的 remote:

```bash
[remote "origin"]
	url = git@github.com:drodata/eo.git
	fetch = +refs/heads/*:refs/remotes/origin/*
```

现在我们将前面创建的 ECS 上的远端也添加进去，`origin` 这个远端名没什么特殊含义，它只是GitHub 上默认的远端名称，我们将 ECS 上的远端的名称命名为`ecs`:

```bash
git remote add ecs git@server:/var/www/blog
```

这样，`eo`就有了两个远端：`origin`(GitHub) 和`ecs`(自己的 ECS 上),
当我们在本地 commit 后，可以使用：

```
git push <remote> <branch>
```

灵活地选择想要 push 的远端和分支，假设这样一个场景：`eo`出现了一个 Bug, 我在本地修复并 commit 后，仅执行

```
git push ecs master
```

就能让服务器段的代码更新。

参考：

- [4.4 Git on the Server - Setting Up the Server](http://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server) 
- https://caiguanhao.wordpress.com/2013/01/10/git-push-non-bare-to-non-bare-repository/ 感谢此文


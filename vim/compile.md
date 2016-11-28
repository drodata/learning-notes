# Compiling Vim

Debian 下默认安装的 Vim 没有开启剪切板功能（`-clipboard`）。要想使用这些额外的功能，就需要重新编译 Vim. 编译大致分为4个步骤。

## 1 安装 Dependencies

```bash
apt-get build-dep vim-gnome
```

这里的 `build-dep` 是 `apt-get` 的一个 option, 地位和 `update`, `install`, `purge` 等相同，作用是安装或删除 package 所需的依赖。

## 2 获取 Vim source code

Vim 的源码在 GitHub 上有托管。我们在本地新建一个目录，然后将源码克隆至本地。

```bash
cd ~/www/repository
git clone git@github.com:vim/vim.git
cd vim
```

> :bell: 注意
> 
> 使用 SSH 从 GitHub 克隆代码仓库的前提是：你有一个 GitHub 账号，且本地主机的 SSH key 在 GitHub 账号上有存储。这里容易忽略的一种情况是——在服务器端（例如国内的 ECS, 国外的 AWS 等）也需要此步骤。

## 3 配置特性列表并 Make

```bash
$ ./configure --with-features=huge
$ make
```

## 4 make install

```bash
sudo make install
```

新版的 Vim 的安装路径为 `/usr/local/bin/vim`，而旧版（Debian 系统自带的版本）的在`/usr/bin/vim`. 由于在环境变量 `$PATH` 的默认设置中，`/usr/local/bing/vim` 的位置比 `/usr/bin/vim` 靠前，所以新版的 Vim 安装好后，直接在命令行输入 `vim`, 将会使用最新版的 Vim.

## 5 简单配置新版的 Vim

1. 新增 `alias vi='vim'`,
2. 解决插入模式下，`<BS>` (退格键)失效的问题
   
   在有缩进的行按`Backspace`并不能删除缩进，只能先退出插入模式，在按`i`重新进入插入模式，这时光标才会到缩进前面。

   解决：在 `.vimrc` 中追加 `set backspace=indent,eol,start` 一行内容即可。

## Reference:

- [Building Vim](http://vim.wikia.com/wiki/Building_Vim)







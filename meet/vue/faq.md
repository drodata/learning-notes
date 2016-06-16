# FAQ

## `npm run dev` 时出现的错误提示

### Error: listen EADDRINUSE :::8080

原因：端口号被占用。解决：更改`./config/index.js` 中端口号配置信息：

```json
module.exports = {
  // ...
  dev: {
    env: require('./dev.env'),
    port: 8088, // change another port number
    proxyTable: {}
  }
}
```

## Syntax Highligting `.vue` files in Vim

借助一款名为 vim-vue 的 Vim 插件，可实现 `.vue` 文件代码高亮。此插件通过 Vim 的插件管理器 Vundle (https://github.com/VundleVim/Vundle.vim) 安装。所以我们先安装 Vundle:

```bash
git clone git@github.com:VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

然后参照官方文档 [Quick Start](https://github.com/VundleVim/Vundle.vim#quick-start) 安装 vim-vue 插件。

如果你使用的是代理上网，在安装插件时有可能发生错误：

![](http://share.drodata.com/wp-content/uploads/2016/06/vundle-error.png)

提示信息如下：

> fatal: unable to access 'https://github.com/posva/vim-vue.git/': 
> Failed to receive SOCKS4 connect request ack.             

此问题的原因尚不清楚，多试几次就可以了。安装成功的提示如下：

![](http://share.drodata.com/wp-content/uploads/2016/06/vundle-install-plugin-ok.png)

此时，打开一个 `.vue` 文件，会发现代码已高亮显示。

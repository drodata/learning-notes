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

https://github.com/VundleVim/Vundle.vim

```bash
git clone git@github.com:VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

> fatal: unable to access 'https://github.com/posva/vim-vue.git/': 
> Failed to receive SOCKS4 connect request ack.             

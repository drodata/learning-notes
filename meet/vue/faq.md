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


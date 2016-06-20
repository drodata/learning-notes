# `router.go(path)`

## 参数

### string | object `path`

如果是字符串，路径中不得含有 dynamic segment 或 star segment. 如果字符串是一个相对路径（开头没有 `/`），则在当前路径下进行解析。

如果是对象， 有两种格式：

- `{path: '...'}`
- 适合 named route
  
  ```js
  {
    name: 'show',
    params: {
      id: 3
    },
    query: {
      action: 'delete'
    }
  }
  ```

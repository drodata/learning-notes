# Vue.js

## Basics

- 命令 `npm run dev` 类似一个 alias, 对应的命令在 `package.json` 内设置：

    ```json
    {
      "scripts": {
        "dev": "node build/dev-server.js",
        "build": "node build/build.js",
        "test": "",
        "lint": "eslint --ext .js,.vue src test/unit/specs test/e2e/specs"
      }
    }
    ```

  从中可看出，执行 `npm run dev` 实际上是在执行命令 `node build/dev-server.js` Ref. [npm-scripts: How npm handles the "scripts" field](https://docs.npmjs.com/misc/scripts)

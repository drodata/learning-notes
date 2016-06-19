# 模板项目

通过 `vue-cli` 创建的模板文件尽管方便，但并不适合初学者。`vue-loader` 文档内有一个 [Basic Tutorial](http://vue-loader.vuejs.org/en/start/tutorial.html), 更适合初学者，但是我自己在实践过程中发现文章中有几处没讲全面，因此完全按照那个文档实践时会发现“跑不起来”。这里，我把文章中的要点复述一遍，并补充缺少的内容。

## 目录结构

一个最简单的 Webpack + `vue-loader` project 的目录结构大致如下：

```
├── components
│   └── App.vue
├── index.html
├── main.js
├── package.json
└── webpack.config.js
```

每个文件的作用：

- `package.json`: Webpack + `vue-loader` 项目本质上是一个 NPM package, 因此，`package.json` 必不可少；
- `webpack.config.js`: Webpack 的配置文件；
- `main.js`: 项目的入口文件（entry script）。Webpack 从读取 `main.js` 文件开始，将各种 JS modules 转换成一个文件 e.g. `bundle.js`.
- `index.html`: 单页应用的唯一入口地址。Webpack 生成的各种 assets 会自动注入到此模板内，生成最终的 HTML 页面
- `components` 目录和其下的各种 `.vue` 文件： Vue.js 中的组件。

## 安装依赖

我们新建一个目录，作为项目的根目录，名字任意取，例如，可以叫 `vue-simple-project`.

在该目录下使用 `npm init` 新建一个 `package.json` 文件。

```bash
$ cd vue-simple-project
# fill in question answers
$ npm init
```

安装所有项目需要的依赖：

```bash
npm install webpack webpack-dev-server vue-loader vue-html-loader css-loader vue-style-loader vue-hot-reload-api babel-loader babel-core babel-plugin-transform-runtime babel-preset-es2015 babel-runtime --save-dev
npm install vue --save
```

一共 13 个依赖！一个最简单的项目都需要这么多！对于首次接触 Vue.js 和 Webpack 的人来说，到这里往往会被吓到。正如原文档中所说，这里的每一个 package 都是必须的。随着学习的深入，我们会发现每一个 package 的作用也很容易理解。

上面的两行命令，安装过程中有可能会出现下面的错误提示（我在运行时就遇到了）：

> error code `EPEERINVALID`
>
> error peerinvalid 
>
> The package vue-hot-reload-api@2.0.1 does not satisfy its siblings' peerDependencies requirements! vue-loader@8.5.2 **wants vue-hot-reload-api@^1.2.0**

解决方法我也总结了，看[这里](/meet/npm/error.md)。

安装好了，我们的 `package.json` 文件中会多出如下内容：

```
  "devDependencies": {
    "babel-core": "^6.9.1",
    "babel-loader": "^6.2.4",
    "babel-plugin-transform-runtime": "^6.9.0",
    "babel-preset-es2015": "^6.9.0",
    "babel-runtime": "^6.9.2",
    "css-loader": "^0.23.1",
    "vue-hot-reload-api": "^1.3.2",
    "vue-html-loader": "^1.2.2",
    "vue-loader": "^8.5.2",
    "vue-style-loader": "^1.0.0",
    "webpack": "^1.13.1",
    "webpack-dev-server": "^1.14.1"
  },
  "dependencies": {
    "vue": "^1.0.25"
  }
```

同时，根目录下多了一个 `node_modules` 目录，我们安装的所有依赖的源文件，就在该目录下。

至此，项目所需的所有依赖已安装完毕。下面我们开始配置 Webpack.

## 配置 Webpack

新建 `webpack.config.js` 文件，内容如下：

```js
module.exports = {
  // entry point of our application
  entry: './main.js',
  // where to place the compiled bundle
  output: {
    path: __dirname,
    filename: 'build.js'
  },
  resolve: {
    extensions: ['', '.js', '.vue']
  },
  module: {
    // `loaders` is an array of loaders to use.
    // here we are only configuring vue-loader
    loaders: [
      {
        test: /\.vue$/, // a regex for matching all files that end in `.vue`
        loader: 'vue'   // loader to use for matched files
      },
      {
        test: /\.js$/,
        loader: 'babel?presets=es2015',
        exclude: /node_modules/
      }
    ]
  }
}
```

相比原文档中的配置，这里增加了两处内容，在文章最后有详细说明。

## 新建其它必要文件

### `main.js`

内容如下：

```js
import Vue from 'vue'
import App from './components/App'

new Vue({
  el: 'body',
  components: {
    app: App
  }
})
```

### `index.html`

内容如下：

```html
<body>
  <app></app>
  <script src="build.js"></script>
</body>
```

### `./components/App.vue`

内容如下：

```
<template>
  <div class="app">
    <app-header></app-header>
    <app-content></app-content>
    <app-footer></app-footer>
  </div>
</template>

<script>
import AppHeader from './AppHeader.vue'
import AppContent from './AppContent.vue'
import AppFooter from './AppFooter.vue'

export default {
  components: {
    AppHeader,
    AppContent,
    AppFooter
  }
}
</script>
```

### `./components/AppHeader.vue`

内容如下：

```
<template>
  <div class="header">
    <h1>{{message}}</h1>
  </div>
</template>

<script>
export default {
    data() {
        return {
            message: 'Hello Vue.js'
        }
    }
}
</script>
```

### `./components/AppContent.vue`

内容如下：

```
<template>
  <div class="content">
    <p>I am the main conent.</p>
  </div>
</template>
```

### `./components/AppFooter.vue`

内容如下：

```
<template>
  <div class="footer">
    <p>I am the footer.</p>
  </div>
</template>
```

## 运行

终于到了运行的时候了！我们使用 NPM scripts 作为运行命令。现在，请在 `package.json` `scripts` option 内增加如下两行命令：

```
{
  ...
  "scripts": {
    ...
    "dev": "webpack-dev-server --inline --hot",
    "build": "webpack -p"
  },
  ...
}
```

现在，在命令行输入 `npm run dev`. 当出现

> webpack: bundle is now VALID.

字样时，在浏览器中打开 `http://localhost:8080`, 就能看到我们的模板项目的真容了！

## 对 `webpack.config.js` 中新增两处配置的说明

相比原文档中的配置，这里增加了两处内容。首先是

```
      {
        test: /\.js$/,
        loader: 'babel?presets=es2015',
        exclude: /node_modules/
      }
```

没有这个，项目在运行时，控制台就会出现如下错误提示：

> Uncaught SyntaxError: Unexpected reserved word

简单说下原因：JS 模块的加载有多种方式，默认的是 CommonJS, 现在流行的是 ES2015 (即 ES6)。前者引入模块使用的是 `require` 关键字，后者使用的是 `import`. 由于我们在 `main.js` 中使用的就是 ES6 语法，因此，必须在 `webpack.config.js` 中指明使用 ES6 去解析我们的 JS 文件，而不使用默认的 CommonJS 方式。

第 2 处新增内容是：

```
  resolve: {
    extensions: ['', '.js', '.vue']
  },
```

缺少这个，在 `npm run dev` 时会提示如下错误：

> ERROR in `./main.js`
>
> Module not found: Error: Cannot resolve 'file' or 'directory' `./components/App` in ...

对应的出错代码行：

```js
// main.js

import App from './components/App'
```

原因： Webpack 默认只会解析 `.js` 文件。而我们要引入的 `./components/App` 是 `.vue` 文件，所以不能识别。`webpack.config.js` 中 `resolve` option 的作用就是告诉 Webpack 也可以识别不带后缀的文件（`''`）.

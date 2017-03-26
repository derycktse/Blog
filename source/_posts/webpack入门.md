---
title: webpack入门
date: 2016-03-10 22:34:59
tags:
---

## 什么是webpack
webpack是一个module bundler，可以将把有依赖关系的各种文件打包成一系列的静态资源
## 安装
首先我们安装最新版的webpack, 我安装的版本是2.2.1
```
npm install -g webpack
```
下面我们将探讨webpack 2 的使用

我们可以直接在终端中使用webpack，命令如下
```
webpack {源文件／入口文件} {目标文件}
webpack --watch //监听模式
webpack -p //混淆脚本   
```

<!--more-->

考虑到随着我们项目复杂度的增高，有可能我们的配置选项也会很多，所以，我们完全可以将配置放到文件中, webpack在执行的时候默认会加载webpack.config.js文件作为配置, 如果我们需要指定配置文件的话，方法如下:
```terminal
webpack --config {配置文件}
```

我们来看下webpack都有哪些配置
```js
//webpack.config.js
const path = require('path');

module.exports = {
  // click on the name of the option to get to the detailed documentation
  // click on the items with arrows to show more examples / advanced options

  entry: "./app/entry", // string | object | array
  // Here the application starts executing
  // and webpack starts bundling

  output: {
    // options related to how webpack emits results

    path: path.resolve(__dirname, "dist"), // string
    // the target directory for all output files
    // must be an absolute path (use the Node.js path module)

    filename: "bundle.js", // string
    // the filename template for entry chunks

    publicPath: "/assets/", // string
    // the url to the output directory resolved relative to the HTML page

    library: "MyLibrary", // string,
    // the name of the exported library

    libraryTarget: "umd", // universal module definition
    // the type of the exported library

    /* Advanced output configuration (click to show) */
  },

  module: {
    // configuration regarding modules

    rules: [
      // rules for modules (configure loaders, parser options, etc.)

      {
        test: /\.jsx?$/,
        include: [
          path.resolve(__dirname, "app")
        ],
        exclude: [
          path.resolve(__dirname, "app/demo-files")
        ],
        // these are matching conditions, each accepting a regular expression or string
        // test and include have the same behavior, both must be matched
        // exclude must not be matched (takes preferrence over test and include)
        // Best practices:
        // - Use RegExp only in test and for filename matching
        // - Use arrays of absolute paths in include and exclude
        // - Try to avoid exclude and prefer include

        issuer: { test, include, exclude },
        // conditions for the issuer (the origin of the import)

        enforce: "pre",
        enforce: "post",
        // flags to apply these rules, even if they are overridden (advanced option)

        loader: "babel-loader",
        // the loader which should be applied, it'll be resolved relative to the context
        // -loader suffix is no longer optional in webpack2 for clarity reasons
        // see webpack 1 upgrade guide

        options: {
          presets: ["es2015"]
        },
        // options for the loader
      },

      {
        test: "\.html$",

        use: [
          // apply multiple loaders and options
          "htmllint-loader",
          {
            loader: "html-loader",
            options: {
              /* ... */
            }
          }
        ]
      },

      { oneOf: [ /* rules */ ] },
      // only use one of these nested rules

      { rules: [ /* rules */ ] },
      // use all of these nested rules (combine with conditions to be useful)

      { resource: { and: [ /* conditions */ ] } },
      // matches only if all conditions are matched

      { resource: { or: [ /* conditions */ ] } },
      { resource: [ /* conditions */ ] }
      // matches if any condition is matched (default for arrays)

      { resource: { not: /* condition */ } }
      // matches if the condition is not matched
    ],

    /* Advanced module configuration (click to show) */
  },

  resolve: {
    // options for resolving module requests
    // (does not apply to resolving to loaders)

    modules: [
      "node_modules",
      path.resolve(__dirname, "app")
    ],
    // directories where to look for modules

    extensions: [".js", ".json", ".jsx", ".css"],
    // extensions that are used

    alias: {
      // a list of module name aliases

      "module": "new-module",
      // alias "module" -> "new-module" and "module/path/file" -> "new-module/path/file"

      "only-module$": "new-module",
      // alias "only-module" -> "new-module", but not "module/path/file" -> "new-module/path/file"

      "module": path.resolve(__dirname, "app/third/module.js"),
      // alias "module" -> "./app/third/module.js" and "module/file" results in error
      // modules aliases are imported relative to the current context
    },
    /* alternative alias syntax (click to show) */

    /* Advanced resolve configuration (click to show) */
  },

  performance: {
    hints: "warning", // enum
    maxAssetSize: 200000, // int (in bytes),
    maxEntrypointSize: 400000, // int (in bytes)
    assetFilter: function(assetFilename) { 
      // Function predicate that provides asset filenames
      return assetFilename.endsWith('.css') || assetFilename.endsWith('.js');
    }
  },

  devtool: "source-map", // enum
  // enhance debugging by adding meta info for the browser devtools
  // source-map most detailed at the expense of build speed.

  context: __dirname, // string (absolute path!)
  // the home directory for webpack
  // the entry and module.rules.loader option
  //   is resolved relative to this directory

  target: "web", // enum
  // the environment in which the bundle should run
  // changes chunk loading behavior and available modules

  externals: ["react", /^@angular\//],
  // Don't follow/bundle these modules, but request them at runtime from the environment

  stats: "errors-only",
  // lets you precisely control what bundle information gets displayed

  devServer: {
    proxy: { // proxy URLs to backend development server
      '/api': 'http://localhost:3000'
    },
    contentBase: path.join(__dirname, 'public'), // boolean | string | array, static file location
    compress: true, // enable gzip compression
    historyApiFallback: true, // true for index.html upon 404, object for multiple paths
    hot: true, // hot module replacement. Depends on HotModuleReplacementPlugin
    https: false, // true for self-signed, object for cert authority
    noInfo: true, // only errors & warns on hot reload
    // ...
  },

  plugins: [
    // ...
  ],
  // list of additional plugins


  /* Advanced configuration (click to show) */
}
```

下面我们来创建一个简单的项目
我们的项目结构如下
```
.
├── dist
├── src
│   └── entry.js
├── index.html
└── webpack.config.js
```

index.html
```
<!DOCTYPE html>
<html>
<head>
	<title></title>
</head>
<body>
	<script src="./dist/bundle.js"></script>
</body>
</html>
```

src/entry.js
```
document.write('hello world')
```

webpack.config.js
```
const path = require('path')

module.exports = {
	entry: './src/entry.js',
	output: {
		path: path.resolve(__dirname, 'dist'),
		filename: 'bundle.js'
	}
}
```

在终端下运行命令:
```
webpack --watch
```

使用[http-server][1]起一个本地服务
```
http-server -p 8080
```
 
可以看到，entry.js的内容被打包到了bundle.js 中
这是webpack最简单的应用，下面我们来看看如何使用loader

## loader
webpack2中已经集成了[json-loader][2], 所以我们无需安装其他的依赖
在src中增加hello.json文件
```json
{"greet" : "hello from json"}
```

修改`entry.js`
```javascript
import json from './hello.json'
document.write(json.greet)
```

刷新http://127.0.0.1:8080/
可以看到hello.json的结果已经输出到浏览器中

#### 使用其他loader
如果其他格式的文件，比如css文件我们可以使用相应的loader来解析,安装依赖
```
npm install --save-dev style-loader css-loader
```
 
修改`webpack.config.js`
```
const path = require('path')

module.exports = {
	entry: './src/entry.js',
	output: {
		path: path.resolve(__dirname, 'dist'),
		filename: 'bundle.js'
	},
	module: {
		rules: [{
			test: /\.css$/,
			use: ["style-loader", "css-loader"]
		}]
	}
}
```
entry.js
```
import './main.css'
import json from './hello.json'
document.write(json.greet)
```
可以看到，main.css也经过loader的解析而打包进bundle.js里面了


  [1]: https://www.npmjs.com/package/http-server
  [2]: https://webpack.js.org/guides/migrating/#json-loader-is-not-required-anymore
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


考虑到随着我们项目复杂度的增高，有可能我们的配置选项也会很多，所以，我们完全可以将配置放到文件中, webpack在执行的时候默认会加载webpack.config.js文件作为配置, 如果我们需要指定配置文件的话，方法如下:
```terminal
webpack --config {配置文件}
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
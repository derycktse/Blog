---
title: webpack使用require.ensure进行代码分割
date: 2016-03-16 04:52:13
tags: [构建]
---

## 代码分割
实例来自于[webpack][1]
假定我们有下面的项目结构
```
.
├── dist
├── js
│   ├── a.js
│   ├── b.js
│   ├── c.js
│   └── entry.js
└── webpack.config.js
```


文件内容
entry.js
```
require('./a');
require.ensure(['./b'], function(require){
    require('./c');
    console.log('done!');
});
```
a.js
```
console.log('***** I AM a *****');
```
b.js
```
console.log('***** I AM b *****');
```
c.js
```
console.log('***** I AM c *****');
```

下面我们看一下`webpack.config.js`的配置
```javascript
const path = require('path')


module.exports =  {
		entry : './js/entry.js'
		,
		output : {
			filename : 'bundle.js'
			, path : path.resolve(__dirname, 'dist')
			, publicPath: './dist/' //当使用代码分割时，publicPath很重要，它将告诉webpack从哪儿去加载其他打包的文件
			, pathinfo : true
		}
	}

```

执行webpack打包之后，我们可以看到结果
![执行webpack打包][2]

我们发现，webpack打包生成了`bundle.js`以及`1.bundle.js`两个文件
查看文件的内容，我们可以发现


```javascript
//bundle.js
/******/ (function(modules) { // webpackBootstrap
/******/ 	/*
				webpack 集成的代码，这里略
			*/
/******/ 	__webpack_require__.p = "./dist/"; //按需加载的路径

/******/ 	// Load entry module and return exports
/******/ 	return __webpack_require__(0);
/******/ })
/************************************************************************/
/******/ ([
/* 0 */
/*!*********************!*\
  !*** ./js/entry.js ***!
  \*********************/
/***/ function(module, exports, __webpack_require__) {

	__webpack_require__(/*! ./a */ 1)

	__webpack_require__.e/* nsure */(1, function (require) {
		__webpack_require__(/*! ./c */ 3)
		console.log('done!')
	})

/***/ },
/* 1 */
/*!*****************!*\
  !*** ./js/a.js ***!
  \*****************/
/***/ function(module, exports) {

	console.log('I am a')

/***/ }
/******/ ]);
```


```javascript
//1.bundle.js
webpackJsonp([1],[
/* 0 */,
/* 1 */,
/* 2 */
/*!*****************!*\
  !*** ./js/b.js ***!
  \*****************/
/***/ function(module, exports) {
	
	console.log('I am b')
/***/ },
/* 3 */
/*!*****************!*\
  !*** ./js/c.js ***!
  \*****************/
/***/ function(module, exports) {
	console.log('I am c')
/***/ }
]);
```
`a.js`的内容被打包到bundle.js之中,而`b.js`,`c.js`则位于`1.bundle.js`中,`b.js`,`c.js`从主入口文件中分离了出来，而且只有`c.js`的内容被执行了


  [1]: https://webpack.js.org/guides/code-splitting-require/
  [2]: https://segmentfault.com/img/bVJS9m?w=1352&h=528
---
title: 三个关于数组的小技巧
date: 2016-04-14 10:36:07
tags: [es2015, javascript]
---

### 1.可迭代的空数组###
我们知道，Javacript的所创建的数组如果未被初始化值的话，它是一个稀疏数组(sparse array)，这是什么意思的？我们先看下面的代码
```js
const arr = new Array(4); //[undefined, undefined, undefined, undefined]
```
假设我们要去迭代一个数组，我们会发现，未被初始化值的数组，index实际上访问不到的：

```js
arr.map((elem, index) => index); //  输出:[undefined, undefined, undefined, undefined]
```
<!--more--> 
我们分别来看[Array.prototype.forEach][1]和[Array.prototype.map][2]的描述，可以留意到：
```
map calls a provided callback function once for each element in an array, in order, and constructs a new array from the results. callback is invoked only for indexes of the array which have assigned values, including undefined. It is not called for missing elements of the array (that is, indexes that have never been set, which have been deleted or which have never been assigned a value).
```
```
forEach() executes the provided callback once for each element present in the array in ascending order. It is not invoked for index properties that have been deleted or are uninitialized (i.e. on sparse arrays).
```
对于`forEach`与`map`这类方法，如果数组有未被初始化值的index, 默认是不回被遍历的。

那么， 如何创建一个所有值默认可被遍历的数组呢？很简单，我们只需要多加一步操作:
```js
const arr = Array.apply(null, new Array(4)); //借助apply给每一个数组的每一个index都赋值
arr.map((elem, index) => index); //这样的话改数组就可以遍历了, 输出 [0, 1, 2, 3]
```
### 2.利用数组给方法传递空参数###
我们调用方法的时候，有时想对一些参数进行忽略，比如下面的情况
```js
method('parameter1', , 'parameter3'); //会报错， Uncaught SyntaxError: Unexpected token ,
```
这个时候通常我们的做法是会传一个`null` 或者 `undefined` 作为占位
```js
method('parameter1', null, 'parameter3')
//或者
method('paramter1', undefined, 'parameter3');
```
我们可以借助ES6的展开语法(...)来辅助传参
```
method(...['parameter1', , 'paramter3']); // 大功告成!
```

### 3. 数组去重###
使用展开符号搭配`set`进行数组去重
```
const arr = [...new Set([1, 2, 3, 3])];//[1, 2, 3]
```

 
参考来源:
 [3 Array Hacks][3]


 [1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach#Description
 [2]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map#Description
 [3]: http://www.jstips.co/en/javascript/3-array-hacks/
    
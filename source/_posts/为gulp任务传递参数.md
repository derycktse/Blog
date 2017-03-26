---
title: 为gulp任务传递参数
date: 2016-03-15 00:26:36
tags: [构建]
---

现在web前端的脚手架中，有相当一部分数量的人选择用gulp做为首选的构建工具，但是，我们是没办法向gulp的任务直接传递命令行参数的。
我们知道gulp实际上是运行在nodejs中的，那么，我们是可以访问`process.argv`
比如命令`gulp task1 --a 123 --b "my string" --c`, 我们看看`process.argv`会输出什么
```js
[
'/usr/bin/nodejs',
'/home/user/.node_modules_global/bin/gulp',
'task1',
'--a',
'123',
'--b',
'my string',
'--c'
]
```
<!--more--> 
既然可以拿到命令行输入的东西，那么我们就可以自己写一个参数解析的函数啦!
```js
// fetch command line arguments
const arg = (argList => {

  let arg = {}, a, opt, thisOpt, curOpt;
  for (a = 0; a < argList.length; a++) {

    thisOpt = argList[a].trim();
    opt = thisOpt.replace(/^\-+/, '');

    if (opt === thisOpt) {

      // argument value
      if (curOpt) arg[curOpt] = opt;
      curOpt = null;

    }
    else {

      // argument name
      curOpt = opt;
      arg[curOpt] = true;

    }

  }

  return arg;

})(process.argv);
console.log(arg) //{ a: '123', b: 'my string', c: true }
```

搞定！



当然，这只是一个简单的实现。
我们还可以使用功能更强大的库, 比如TJ大神的[commander][2]

参考
[How to Pass Command Line Parameters to Gulp Tasks][1]

[1]: https://www.sitepoint.com/pass-parameters-gulp-tasks/
[2]: https://github.com/tj/commander
---
title: vue
date: 2017-04-05 14:48:30
tags:
---

### [83fac017f96f34c92c3578796a7ddb443d4e1f17] ###

使用grunt作为构建工具
npm + component作为包的管理

### [871ed9126639c9128c18bb2f19e6afd42c0c5ad9] ###
新增加了一个数据绑定的实例
大致思路：
- html结构中新增占位符号
- 将有占位符号的DOM保存起来，同时拥有一个对象将DOM关联起来
- 使用Object.defineProperty为这个对象的属性进行绑定访问器，如果修改了这个访问器的话，也将更改更新到DOM中 

[实例地址](https://github.com/vuejs/vue/blob/871ed9126639c9128c18bb2f19e6afd42c0c5ad9/explorations%2Fgetset.html)


### [a5e27b1174e9196dcc9dbb0becc487275ea2e84c] ###
作者同时使用了compoent的包管理工具
看提交时间必须使用component@0.16.3这个版本
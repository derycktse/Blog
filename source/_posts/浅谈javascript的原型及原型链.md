---
title: 浅谈javascript的原型及原型链
date: 2016-03-08 22:26:51
tags: [javascript, prototype]
---
原型与原型链是javascript里面最最核心的内容，如果不能理解它们之间的存在关系的话，那么我们是不能理解这门语言的。

在JS中，主要有两种创建对象的方法, 分别是对象字面量以及调用构造函数
```javascript
//对象字面量
var obj1 = {}

//调用构造函数
var obj2 = new Object()
```

其实上述两种创建对象的方法，本质上是一样的，都是JS引擎调用对象的构造函数来新建出一个对象。构造函数本身也是一个普通的JS函数

下面我们来看一个例子
```javascript
//创建构造函数
function Person(name){
    this.name = name
}

//每个构造函数JS引擎都会自动添加一个prototype属性，我们称之为原型，这是一个对象
//每个由构造函数创建的对象都会共享prototype上面的属性与方法
console.log(typeof Person.prototype) // 'object'


//我们为Person.prototype添加sayName方法
Person.prototype.sayName = function(){
    console.log(this.name)
}

//创建实例
var person1 = new Person('Messi')
var person2 = new Person('Suarez')

person1.sayName() // 'Messi'
person2.sayName() // 'Suarez'

person1.sayName === person2.sayName //true
```

我们借助上面的例子来理解构造函数-原型-实例，三者之间的关系，主要有几个基本概念
- 构造函数默认会有一个`protoype`属性指向它的原型
- 构造函数的原型会有一个`consctructor`的属性指向构造函数本身, 即
    ```javascript
        Person.prototype.constructor === Person
    ```
- 每一个`new`出来的实例都有一个隐式的`__proto__`属性，指向它们的构造函数的原型，即
    ```javascript
    person1.__proto__ === Person.prototype
    person1.__proto__.constructor === Person
    ```

了解了这些基本概念之后，我们再来看看javascript的一些原生构造函数的关系网，看下列的图

![javascript objects treasure map](https://i.stack.imgur.com/KFzI3.png)
引自[stackoverflow][1]

按照我们上面的理解, Oject本身是一个构造函数，它也是一个对象，那么
    ```javascript
        Object.__proto__ === Function.prototype
    ```

为了方便我们记住上图，还有几个需要我们知道的特殊概念：

- `Function`的原型属性与`Function`的原型指向同一个对象. 即
    ```javascript 
        Function.__proto__ == Function.prototype
    ```
- `Object.prototype.__proto__ === null`
- `typeof Function.prototype === 'function'`


  [1]: http://stackoverflow.com/questions/650764/how-does-proto-differ-from-constructor-prototype
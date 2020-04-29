---
title: Javascript原型的深刻剖析
urlname: wpcf0g
date: 2019-08-02 11:47:08 +0800
tags: []
categories: []
---

[原文](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
对于使用过基于类的语言 (如 Java 或 C++) 的开发人员来说，JavaScript 有点令人困惑，因为它是动态的，并且本身不提供一个  `class`  实现。（在 ES2015/ES6 中引入了  `class`  关键字，但那只是语法糖，JavaScript 仍然是基于原型的）。
当谈到继承时，JavaScript 只有一种结构：对象。每个实例对象（ object ）都有一个私有属性（称之为 **proto** ）指向它的构造函数的原型对象（**prototype **）。该原型对象也有一个自己的原型对象( **proto** ) ，层层向上直到一个对象的原型对象为  `null`。根据定义，`null`  没有原型，并作为这个**原型链**中的最后一个环节。
几乎所有 JavaScript 中的对象都是位于原型链顶端的  [`Object`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)  的实例。
尽管这种原型继承通常被认为是 JavaScript 的弱点之一，但是原型继承模型本身实际上比经典模型更强大。例如，在原型模型的基础上构建经典模型相当简单。

不懂没关系，看图
![image.png](https://cdn.nlark.com/yuque/0/2019/png/247878/1564717647209-0d719ed4-0f99-4d9d-9ab2-5b14ea1b404e.png#align=left&display=inline&height=243&name=image.png&originHeight=243&originWidth=876&size=33411&status=done&width=876)

那么 Javascript 的逻辑结构其实就清晰了：我拿 window 举个栗子
null->{constructor....}->EventTaget->WindowProperties->Window->window

而真正的数据结构是反过来的：
我们把箭头方向倒过来就好了，原理也是很简单.

```javascript
function doSomething() {}
doSomething.prototype.foo = "bar"; // add a property onto the prototype
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value"; // add a property onto the object
console.log(doSomeInstancing);
```

返回结果：

```
{
    prop: "some value",
    __proto__: {
        foo: "bar",
        constructor: ƒ doSomething(),
        __proto__: {
            constructor: ƒ Object(),
            hasOwnProperty: ƒ hasOwnProperty(),
            isPrototypeOf: ƒ isPrototypeOf(),
            propertyIsEnumerable: ƒ propertyIsEnumerable(),
            toLocaleString: ƒ toLocaleString(),
            toString: ƒ toString(),
            valueOf: ƒ valueOf()
        }
    }
}
```

###

### 总结：4 个用于拓展原型链的方法

下面列举四种用于拓展原型链的方法，以及他们的优势和缺陷。下列四个例子都创建了完全相同的  `inst`  对象（所以在控制台上的输出也是一致的），为了举例，唯一的区别是他们的创建方法不同。

| 名称               | 例子 | 优势 | 缺陷 |
| :----------------- | :--- | :--- | :--- |
| New-initialization | ```  |

function foo(){}
foo.prototype = {
foo_prop: "foo val"
};
function bar(){}
var proto = new foo;
proto.bar_prop = "bar val";
bar.prototype = proto;
var inst = new bar;
console.log(inst.foo_prop);
console.log(inst.bar_prop);

````
 | 支持目前以及所有可想象到的浏览器(IE5.5都可以使用). 这种方法非常快，非常符合标准，并且充分利用JIST优化。 | 为使用此方法，这个问题中的函数必须要被初始化。 在这个初始化过程中，构造可以存储一个唯一的信息，并强制在每个对象中生成。但是，这个一次性生成的独特信息，可能会带来潜在的问题。另外，构造函数的初始化，可能会给生成对象带来并不想要的方法。 然而，如果你只在自己的代码中使用，你也清楚（或有通过注释等写明）各段代码在做什么，这些在大体上都根本不是问题（事实上，还常常是有益处的）。 |
| Object.create | ```
function foo(){}
foo.prototype = {
  foo_prop: "foo val"
};
function bar(){}
var proto = Object.create(
  foo.prototype
);
proto.bar_prop = "bar val";
bar.prototype = proto;
var inst = new bar;
console.log(inst.foo_prop);
console.log(inst.bar_prop);
````

```
function foo(){}
foo.prototype = {
  foo_prop: "foo val"
};
function bar(){}
var proto = Object.create(
  foo.prototype,
  {
    bar_prop: {
      value: "bar val"
    }
  }
);
bar.prototype = proto;
var inst = new bar;
console.log(inst.foo_prop);
console.log(inst.bar_prop)
```

| 支持当前所有非微软版本或者 IE9 以上版本的浏览器。允许一次性地直接设置  `__proto__`  属性，以便浏览器能更好地优化对象。同时允许通过  `Object.create(null)`来创建一个没有原型的对象。 | 不支持 IE8 以下的版本。然而，随着微软不再对系统中运行的旧版本浏览器提供支持，这将不是在大多数应用中的主要问题。 另外，这个慢对象初始化在使用第二个参数的时候有可能成为一个性能黑洞，因为每个对象的描述符属性都有自己的描述对象。当以对象的格式处理成百上千的对象描述的时候，可能会造成严重的性能问题。 |
| Object.setPrototypeOf | ```
function foo(){}
foo.prototype = {
foo_prop: "foo val"
};
function bar(){}
var proto = {
bar_prop: "bar val"
};
Object.setPrototypeOf(
proto, foo.prototype
);
bar.prototype = proto;
var inst = new bar;
console.log(inst.foo_prop);
console.log(inst.bar_prop);

```

```

function foo(){}
foo.prototype = {
foo_prop: "foo val"
};
function bar(){}
var proto;
proto=Object.setPrototypeOf(
{ bar_prop: "bar val" },
foo.prototype
);
bar.prototype = proto;
var inst = new bar;
console.log(inst.foo_prop);
console.log(inst.bar_prop)

````
 | 支持所有现代浏览器和微软IE9+浏览器。允许动态操作对象的原型，甚至能强制给通过 `Object.create(null) `创建出来的没有原型的对象添加一个原型。 | 这个方式表现并不好，应该被弃用。如果你在生产环境中使用这个方法，那么快速运行 Javascript 就是不可能的，因为许多浏览器优化了原型，尝试在调用实例之前猜测方法在内存中的位置，但是动态设置原型干扰了所有的优化，甚至可能使浏览器为了运行成功，使用完全未经优化的代码进行重编译。 不支持 IE8 及以下的浏览器版本。 |
| __proto__ | ```
function foo(){}
foo.prototype = {
  foo_prop: "foo val"
};
function bar(){}
var proto = {
  bar_prop: "bar val",
  __proto__: foo.prototype
};
bar.prototype = proto;
var inst = new bar;
console.log(inst.foo_prop);
console.log(inst.bar_prop);
````

```
var inst = {
  __proto__: {
    bar_prop: "bar val",
    __proto__: {
      foo_prop: "foo val",
      __proto__: Object.prototype
    }
  }
};
console.log(inst.foo_prop);
console.log(inst.bar_prop)
```

| 支持所有现代非微软版本以及 IE11 以上版本的浏览器。将  `__proto__`  设置为非对象的值会静默失败，并不会抛出错误。 | 应该完全将其抛弃因为这个行为完全不具备性能可言。 如果你在生产环境中使用这个方法，那么快速运行 Javascript 就是不可能的，因为许多浏览器优化了原型，尝试在调用实例之前猜测方法在内存中的位置，但是动态设置原型干扰了所有的优化，甚至可能使浏览器为了运行成功，使用完全未经优化的代码进行重编译。不支持 IE10 及以下的浏览器版本。 |

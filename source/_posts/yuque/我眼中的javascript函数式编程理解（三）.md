---
title: 我眼中的javascript函数式编程理解（三）
urlname: arg2mp
date: 2019-08-02 17:39:05 +0800
tags: []
categories: []
---

## 不可或缺的 curry

（译者注：原标题是“Can't live if livin' is without you”，为英国乐队 Badfinger 歌曲  *Without You*  中歌词。）
我父亲以前跟我说过，有些事物在你得到之前是无足轻重的，得到之后就不可或缺了。微波炉是这样，智能手机是这样，互联网也是这样——老人们在没有互联网的时候过得也很充实。对我来说，函数的柯里化（curry）也是这样。
curry 的概念很简单：只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
你可以一次性地调用 curry 函数，也可以每次只传一个参数分多次调用。

emmm 这里举个例子奥
正常我们写的话：

```javascript
function a(val1，callback){
 		console.log(val1);
    callback();
}
```

为了 curry 而 curry 的写法。。。

```javascript
function a(val1) {
  return function (callback) {
    console.log(val1);
    callback();
  };
}
//函数式一点的写法
const a = (val1) => (callback) => {
  console.log(val1);
  callback();
};
a(1111)(fn);
```

那么更好用的写法：

```javascript
const fn = (a) => (b) => fn2(a, b);
```

done!

---
title: 我眼中的javascript函数式编程理解（一）
urlname: qk5s79
date: 2019-08-02 16:30:51 +0800
tags: []
categories: []
---

最简单的:

```javascript
const restult = (x) => (y) => x + y;
```

那么他描述了一个什么事情呢？
result=x+y;

我们这里函数式编程就是很简单的理解为 结果=f(输入)；

那么为啥要这样写呢？
我们举个例子奥：

```javascript
const hi = (name) => `Hi ${name}`;
const greeting = (name) => hi(name);
```

这个代码满足了函数式编程的要求，然后我们发现我们其实是可以化简这个代码的！
这跟数学公式一样将式 1 带入式 2 那么就有

```javascript
const hi = (name) => `Hi ${name}`;
const greeting = (name) => (name) => `Hi ${name}`;
//整理
const greeting = (name) => `Hi ${name}`;
```

最后就是一个式子：
const greeting= name=>`Hi ${name}`;

如果你认为上面的代码不能体现出函数式编程的优势我们再来一个例子：
首先我们写一个非函数式编程的代码

```javascript
//非函数式编程形式
const getServerStuff = (callback) => {
  return ajaxCall((json) => callback(json));
};
```

我们将它化简，转化为函数式编程模式

```javascript
const getServerStuff = (callback) => ajaxCall((json) => callback(json));
```

现在我们再看这个代码我们继续化简

```javascript
const getServerStuff = (callback) => ajaxCall(callback);
//那么就会出现
const getServerStuff = ajaxCall;
```

这个代码事实上什么也没干跟上面,为什么这么说呢我们先按照函数式编程的思维思考：
他们为什么相等呢？
因为他们的输出入输出都相同！

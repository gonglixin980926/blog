---
title: redux 快速上手
urlname: gcqofl
date: 2019-09-29 21:30:57 +0800
tags: []
categories: []
---

redux 官方文档，实例的 todo 看的我脑壳疼，各种查找，搜索，发现这个真难理解，为什么呢？因为他们都把这三个拆开介绍，本人本来就对这个玩意陌生，结果还拆开。
瞬间懵逼。

事实上 redux 实际开发上手非常简单。我们走起~~~

首先介绍一下，只需三行就能完事~~。

state 这个就是跟 react 里面的 state 一样，跟 vue 的 data 一样，存储数据（不过跟 react 一个尿性只能手动更新）

store 因为 state 不能直接赋值，所有要调用 store 的函数去更新和获取值。

action 通过 action 去操作 state 从而达到改变 state 的一个具体实现 action 可以是 obj 也可以是 function。

我们需要做的很简单

```javascript
//创建一个对象用来初始 state
let initStates = {
  life: "bad",
};

function reducer(state = initStates, action) {
  //在这里面可以各种操作从而返回改变好的state
  if (action.type === "changeLife") {
    state.life = action.life;
  }

  return state;
}
//创建一个store
let store = createStore(reducer);
store.getState().life; //这就拿到了初始的life
//现在redux 就完成最基本的初始化了，如果不理解，那不要紧
//我们写给action去改变一下state的值
let dispatchLife = {
  type: "changeLife",
  life: "good",
};
store.dispatch(dispatchLife);
//在dispatch那一刻life 从bad 变成 good 了
//获取也很简单，只需要
store.getState().life; //这就拿到了改变的life
```

那么现在你再去看官网的那些概念是不是就很好理解了呢~~~

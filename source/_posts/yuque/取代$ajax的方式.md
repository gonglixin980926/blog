---
title: 取代$ajax的方式
urlname: wuyg0x
date: 2019-06-30 23:13:49 +0800
tags: []
categories: []
---

原生

```javascript
fetch(url)
  .then((response) => response.json())

  .then((data) => console.log(data))

  .catch((e) => console.log("Oops, error", e));
```

兼容的话就是用 xmlhttprequest 实现 promise 方法，GitHub 有这个，自己也可以简化。

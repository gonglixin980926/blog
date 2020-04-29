---
title: vh,vh适配的方法
urlname: ggpfln
date: 2019-07-03 19:16:12 +0800
tags: []
categories: []
---

将视口宽度 `window.innerWidth` 和视口高度 `window.innerHeight` 等分为 100 份，且将这里的视口理解成 idealviewport 更为贴切，并不会随着 viewport 的不同设置而改变。

- vw : 1vw 为视口宽度的 1%
- vh : 1vh 为视口高度的 1%
- vmin : vw 和 vh 中的较小值
- vmax : 选取 vw 和 vh 中的较大值

如果设计稿为 750px，那么 1vw = 7.5px，100vw = 750px。其实设计稿按照设么都没多大关系，最终转化过来的都是相对单位，上面讲的 rem 也是对它的模拟。这里的比例关系也推荐不要自己换算，使用 pxtoviewport 的库就可以帮我们转换。当然每种方案都会有其弊端，这里就不展开讨论。

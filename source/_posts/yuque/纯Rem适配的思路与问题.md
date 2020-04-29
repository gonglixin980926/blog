---
title: 纯Rem适配的思路与问题
urlname: ehnuw3
date: 2019-07-03 19:09:01 +0800
tags: []
categories: []
---

适配方案 1

```javascript
//移动垂直屏幕适配方案
//默认设计稿宽度为750px
//1rem=100px
var initScreen = function () {
  var designWidth = 750;
  var _w =
    window.innerWidth ||
    document.documentElement.clientWidth ||
    document.body.clientWidth ||
    window.screen.width;
  if ($("body").width() > 0) {
    $("html").css("font-size", ($("body").width() / designWidth) * 625 + "%");
  } else if (_w > 0) {
    $("html").css("font-size", (_w / designWidth) * 625 + "%");
  }
  $(window).resize(function () {
    $("html").css("font-size", ($("body").width() / designWidth) * 625 + "%");
  });
};
```

适配方案 2
通过对 initial-scale = 1/dpr 的设置，已将对屏幕的描述从物理像素转化到了物理像素上了，这将是后续推导的基础，且设计稿为 750px。

1. 物理像素为 750 = 375 \* 2，若屏幕等分为 10 份，那么 1rem = 75px，10rem = 750px;
1. 物理像素为 1125 = 375 \* 3，若屏幕等分为 10 份，那么 1rem = 112.5px, 10rem = 1125px;
1. 物理像素为 1242 = 414 \* 3, 若屏幕等分为 10 份，那么 1rem = 124.2px, 10rem = 1242px;

因此可推导出 rem 的设定方式：

```javascript
document.documentElement.style.fontSize =
  document.documentElement.clientWidth / 10 + "px";
```

下面我们将 750px 下，1rem 代表的像素值用 baseFont 表示，则在 baseFont = 75 的情况下，是分成 10 等份的。因此可以将上面的公式通用话一些：

```javascript
document.documentElement.style.fontSize =
  document.documentElement.clientWidth / (750 / 75) + "px";
```

整体设置可参考如下代码：

```javascript
(function (baseFontSize) {
  const _baseFontSize = baseFontSize || 75;
  const ua = navigator.userAgent;
  const matches = ua.match(/Android[\S\s]+AppleWebkit\/(\d{3})/i);
  const isIos = navigator.appVersion.match(/(iphone|ipad|ipod)/gi);
  const dpr = window.devicePixelRatio || 1;
  if (!isIos && !(matches && matches[1] > 534)) {
    // 如果非iOS, 非Android4.3以上, dpr设为1;
    dpr = 1;
  }
  const scale = 1 / dpr;
  const metaEl = document.querySelector('meta[name="viewport"]');
  if (!metaEl) {
    metaEl = document.createElement("meta");
    metaEl.setAttribute("name", "viewport");
    window.document.head.appendChild(metaEl);
  }
  metaEl.setAttribute(
    "content",
    "width=device-width,user-scalable=no,initial-scale=" +
      scale +
      ",maximum-scale=" +
      scale +
      ",minimum-scale=" +
      scale
  );

  document.documentElement.style.fontSize =
    document.documentElement.clientWidth / (750 / _baseFontSize) + "px";
})();
```

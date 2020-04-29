---
title: viewport适配思路（目前我没看懂）
urlname: iqanud
date: 2019-09-12 11:26:39 +0800
tags: []
categories: []
---

```javascript
//移动端版本兼容

var jsVer = 15;
var phoneWidth = parseInt(window.screen.width);
var phoneScale = phoneWidth / 640;

var ua = navigator.userAgent;
if (/Android (\d+\.\d+)/.test(ua)) {
  var version = parseFloat(RegExp.$1);
  // andriod 2.3
  if (version > 2.3) {
    document.write(
      '<meta name="viewport" content="width=640, minimum-scale = ' +
        phoneScale +
        ", maximum-scale = " +
        phoneScale +
        ', target-densitydpi=device-dpi">'
    );
    // andriod 2.3以上
  } else {
    document.write(
      '<meta name="viewport" content="width=640, target-densitydpi=device-dpi">'
    );
  }
  // 其他系统
} else {
  document.write(
    '<meta name="viewport" content="width=640, user-scalable=no, target-densitydpi=device-dpi">'
  );
}
```

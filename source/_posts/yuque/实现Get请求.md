---
title: 实现Get请求
urlname: psd3d0
date: 2019-07-03 19:35:58 +0800
tags: []
categories: []
---

```javascript
function getJSON(url) {
  return new Promise(function (resolve, reject) {
    var XHR = new XMLHttpRequest();
    XHR.open("GET", url, true);
    XHR.send();
    XHR.onreadystatechange = function () {
      if (XHR.readyState == 4) {
        if (XHR.status == 200) {
          try {
            var response = JSON.parse(XHR.responseText);
            resolve(response);
          } catch (e) {
            reject(e);
          }
        } else {
          reject(new Error(XHR.statusText));
        }
      }
    };
  });
}
//用法
getJSON(url).then((resp) => console.log(resp));
```

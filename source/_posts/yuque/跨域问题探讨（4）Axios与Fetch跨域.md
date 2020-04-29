---
title: 跨域问题探讨（4）Axios与Fetch跨域
urlname: xtogc9
date: 2020-04-14 21:54:37 +0800
tags: []
categories: []
---

JSONP 跨域其实就是利用了 src 标签的特性
看见这代码：

```javascript
axios({
  method: "post",
  headers: {
    Authorization: "Basic QXBpQm9vdDpBcGlCb290U2VjcmV0",
  },
  url: `http://localhost:9000/oauth/token?grant_type=password&username=${_values.username}&password=${_values.password}`,
})
  .then((data) => {
    console.log(data);
    message.success("登录成功，正在为您跳转...", 1);
    // @ts-ignore
    this.props.history.push("/");
  })
  .catch((error) => {
    message.error("用户密码错误，或者账户不存在", 3);
  });
```

跨域了，但是没做处理，很简单，因为我在 springboot 里面配置了。
fetch 也同理，就不写了。

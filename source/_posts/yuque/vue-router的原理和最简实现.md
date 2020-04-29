---
title: vue-router的原理和最简实现
urlname: cmk1by
date: 2019-07-31 14:10:03 +0800
tags: []
categories: []
---

vue javascript vue-router

Vue 的机制和 react 有很大差别，他的 router 的设计也是足够奇思妙想，而且符合编程逻辑

先从配置说起，要在 webpack 环境下读取 vue 文件必须安装 vue-loader 插件![image.png](https://cdn.nlark.com/yuque/0/2019/png/247878/1564553659698-52bf9947-1d61-4b58-9a9c-b353f79ae39c.png#align=left&display=inline&height=341&name=image.png&originHeight=341&originWidth=383&size=14201&status=done&width=383)
这样配置即可。
之后开始搞事情~~~

模仿官方正版的 vue-router 的文件结构，我配置了一个 vue-router 类似的 routers.js

```javascript
export default {
  "/": "Home",
  "/about": "About",
};
```

然后在 main.js 里进行注入：
下面看我的注释来讲解这些代码的具体含义

```javascript
import Vue from "vue";
import routes from "./routes";

const app = new Vue({
  el: "#app",
  data: {
    //初始化当前路由位置
    currentRoute: window.location.pathname,
  },
  computed: {
    //在vue的computed函数中进行逻辑计算
    ViewComponent() {
      // 在刚才定义的routers文件里面找是否有带当前路径的router注册，有就会返回routes[this.currentRoute]的值，没有就会返回undefined
      const matchingView = routes[this.currentRoute];
      //这里跟vue原版的router不一样的是它在这里请求组件的，那我们也可以改成和原版一样的模式等会看下面
      return matchingView
        ? require("./pages/" + matchingView + ".vue")
        : require("./pages/404.vue");
    },
  },
  render(h) {
    //那么现在返回的就是最关键的组件，这里面只有一个vue实例，这就是传说中的vue单页应用
    //但是我们也可以开多个实例。
    return h(this.ViewComponent);
  },
});
//这里我们看到了一个非常有意思的写法，这里是用来监控路径发生变化的，具体看下面的链接
window.addEventListener("popstate", () => {
  app.currentRoute = window.location.pathname;
});
```

[对 popstate 事件的介绍和使用方法](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/popstate_event)

下面，我们将在官方给的示例的基础上继续推进，争取还原。

```javascript
import Home from "./pages/Home.vue";
import About from "./pages/About.vue";

let routes = [
  {
    path: "/",
    name: "home",
    component: Home,
  },
  {
    path: "/about",
    name: "about",
    component: About,
  },
];
export default routers;
```

```javascript
import Vue from "vue";
import routes from "./routes";

const app = new Vue({
  el: "#app",
  data: {
    currentRoute: window.location.pathname,
  },
  computed: {
    ViewComponent() {
      for (var i = 0; i < routes.length; i++) {
        if (routes[i].path == this.currentRoute) {
          return routes[i].component;
        } else {
          return Erro;
        }
      }
    },
  },
  render(h) {
    return h(this.ViewComponent);
  },
});

window.addEventListener("popstate", () => {
  app.currentRoute = window.location.pathname;
});
```

这个完成的其实也不是很漂亮，正常应该在 vue 里面去接受 router，然后再在单独的文件去修改，做到解耦。

最后：
[参照思路](https://github.com/chrisvfritz/vue-2.0-simple-routing-example)

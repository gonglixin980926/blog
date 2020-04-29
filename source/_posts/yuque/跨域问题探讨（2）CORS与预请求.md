---
title: 跨域问题探讨（2）CORS与预请求
urlname: uxyd6l
date: 2020-04-14 21:34:05 +0800
tags: []
categories: []
---

跨域资源共享([CORS](https://developer.mozilla.org/zh-CN/docs/Glossary/CORS)) 是一种机制，它使用额外的 [HTTP](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP) 头来告诉浏览器   让运行在一个 origin (domain) 上的 Web 应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的**域**、**协议**或**端口**请求一个资源时，资源会发起一个跨域 HTTP 请求。
比如，站点  [http://domain-a.com](http://domain-a.com)  的某 HTML 页面通过 [<img> 的 src ](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Img#Attributes)请求  [http://domain-b.com/image.jpg](http://domain-b.com/image.jpg)。网络上的许多页面都会加载来自不同域的 CSS 样式表，图像和脚本等资源。
出于安全原因，浏览器限制从脚本内发起的跨源 HTTP 请求。 例如，XMLHttpRequest 和 Fetch API 遵循同源策略。 这意味着使用这些 API 的 Web 应用程序只能从加载应用程序的同一个域请求 HTTP 资源，除非响应报文包含了正确 CORS 响应头。
  （译者注：这段描述不准确，并不一定是浏览器限制了发起跨站请求，也可能是跨站请求可以正常发起，但是返回结果被浏览器拦截了。）
![](https://cdn.nlark.com/yuque/0/2020/png/247878/1586871366600-3882c57c-bede-421b-9321-e7a1bc1dba72.png#align=left&display=inline&height=306&margin=%5Bobject%20Object%5D&originHeight=643&originWidth=925&size=0&status=done&style=none&width=440)
跨域资源共享（ [CORS](https://developer.mozilla.org/zh-CN/docs/Glossary/CORS) ）机制允许  Web 应用服务器进行跨域访问控制，从而使跨域数据传输得以安全进行。现代浏览器支持在 API 容器中（例如 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 或 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) ）使用 CORS，以降低跨域 HTTP 请求所带来的风险。

**1.其中简单请求不会触发预检机制**
\*\*
**2.只有复杂的就是带自定义请求头的请求才会触发预检机制。**
\*\*
**3.如果预检返回状态不是 200 ok 那么就会触发跨域保护**

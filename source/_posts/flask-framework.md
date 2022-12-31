---
title: flask-framework
date: 2020-11-16 11:25:25
tags:
categories:
description:
photos:
---

# Flask框架升级研究

[toc]

## WEB是什么

Web应用的本质就是：

1. 浏览器发送一个**HTTP请求**；
2. 服务器收到请求，生成一个**HTML文档**；
3. 服务器把HTML文档作为HTTP响应的**Body**发送给浏览器；
4. 浏览器收到HTTP响应，从HTTP Body取出HTML文档并显示。

**接受HTTP请求、解析HTTP请求、发送HTTP响应都是苦力活**，如果我们自己来写这些底层代码，还没开始写动态HTML呢，就得花个把月去读HTTP规范。

正确的做法是**底层代码由专门的服务器软件实现**，**我们用Python专注于生成HTML文档**

> WSGI规定了这些底层代码的规范 有很多项目组实现了这个规范 编写了很多服务器  例如：？？
>
> WSGI是说明书 ？？是依据说明书生产出来的机器

## 参考链接

【1】[WEB开发入门介绍](https://www.liaoxuefeng.com/wiki/1016959663602400/1017805733037760)

【2】[python web部署整体介绍](http://www.nowamagic.net/academy/part/13/302)
---
title: 扒一扒浏览器缓存机制
date: 2019-02-07 20:05:41
tags: [基础,浏览器]
---

## 前言

[原文](https://jayzangwill.github.io/blog/2018/11/24/line-height-and-vertical-align/) && [个人主页](https://www.jayzangwill.cn)
[知乎](https://zhuanlan.zhihu.com/p/51189193) && [知乎专栏](https://zhuanlan.zhihu.com/jayzangwill)

## 背景

之所以会写这篇文章，是因为之前在工作中发生的一次微信浏览器莫名缓存的问题。这个问题的源头是因为万恶的微信浏览器将页面的入口文件给缓存起来了（单页面应用），因此，通过写下这篇文章来理一理浏览器的缓存机制。

详细的解决方案可以看[这里](https://www.jianshu.com/p/cce9511c0914)

<!-- more -->

## 浏览器的缓存机制

既然浏览器需要缓存资源，就得知道资源啥时候更新。要想知道资源啥时候更新，这就得引出两个概念：**强制缓存**和**协商缓存**

### 强制缓存

当命中强制缓存后，浏览器不会去请求服务器，而是直接使用本地缓存的资源，并返回状态码`200(缓存来源)`。是否命中强制缓存是由响应头中的`expires`和`Cache-Control`控制（浏览器的第一个请求不会命中强制缓存，Service Worker除外，感兴趣的同学可以用node试一试）。

响应头如图：![响应头](/blog/img/cache/header_1.png)

当命中强制缓存的时候返回的状态码：![强制缓存状态码](/blog/img/cache/code_200.png)

#### expires

`expires`是http1.0的标准，表明服务器的过期时间，是格林威治时间，当请求的时候客户端的时间超过`expires`标识的时间时，就会去请求服务器。

可以看到，**当请求的时候客户端的时间超过`expires`标识的时间时，就会去请求服务器**，但是这个其实是存在问题的，当用户的系统时间改到这个标识的时间之后，就永远不会命中这个强制缓存。所以`Cache-Control`就诞生了。

#### Cache-Control

`Cache-Control`是http1.1的产物，他可以看成是`expries`的补充。

介绍一些`Cache-Control`常用的值：

1. max-age: 设置缓存的最大的有效时间，单位为秒（s）。max-age会覆盖掉expires。
2. s-maxage: 设置代理服务器缓存的最大的有效时间，单位为秒（s）。s-maxage会覆盖掉max-age。
3. public: 表明响应可以被任何对象（包括：发送请求的客户端，代理服务器，等等）缓存。
4. private: 只有发起请求的浏览器可进行缓存。
5. no-cache: 虽然字面意义是“不要缓存”。但它实际上的机制是，仍然对资源使用缓存，但每一次在使用缓存之前必须向服务器对缓存资源进行验证。
6. no-store: 所有内容都不会被缓存，即不使用强制缓存，也不使用协商缓存。
7. must-revalidate: 如果你配置了max-age信息，当缓存资源仍然新鲜（小于max-age）时使用缓存，否则需要对资源进行验证

所以判断缓存是否过期步骤是：

1. 查看是否有cache-control的max-age / s-maxage，如果有，则用服务器时间date值 + max-age/s-maxage 的秒数计算出新的过期时间，将当前时间与过期时间进行比较，判断是否过期。如果没有则用expires 作为过期时间比较。

![步骤图](/blog/img/cache/step.png)

#### 缓存的位置

关于缓存位置可以查看[这篇文章](https://www.jianshu.com/p/54cc04190252)

总的来说就分为四个位置：

1. [Service Worker](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API/Using_Service_Workers)
2. Memory Cache
3. Disk Cache
4. Push Cache

### 小结

浏览器的强制缓存由响应头中的`expires`和`Cache-Control`控制。当`Cache-Control`中有max-age/s-maxage时则用服务器时间date值 + max-age/s-maxage 的秒数计算出新的过期时间；否则用`expires`作为资源的过期时间。

#### 协商缓存

顾名思义，协商缓存就是得与服务器商量一下是否使用缓存，命中协商缓存的话会返回状态`304`。

协商缓存由响应头中的`Last-Modified`和`ETag`还有请求头中的`If-Modified-Since`和`If-None-Match`控制。

如图：![响应头和请求头](/blog/img/cache/header_2.png)

那么这四个东西是什么玩意呢？接下来就由我细细道来：

**Last-Modified**：标示这个响应资源的最后修改时间。

**If-Modified-Since**：当资源过期时（使用`Cache-Control`标识的max-age/s-maxage），发现资源具有`Last-Msodified`声明，则向服务器请求时带上头 `If-Modified-Since`（即响应头中的`Last-Modified`值），表示请求时间。这个时候服务器收到请求后发现有头`If-Modified-Since`则与**被请求资源**的最后修改时间进行比对。若最后修改时间较新，说明资源有被改动过，将新资源返回并返回状态`200`，否则返回`304`表示资源没被更新使用缓存即可。

**Etag**：服务器响应请求时，告诉浏览器当前资源在服务器的唯一标识（生成规则由服务器决定）。例如在Apache中，`ETag`的值，默认是对文件的索引节（INode），大小（Size）和最后修改时间（MTime）进行Hash后得到的。

**If-None-Match**：当资源过期时（使用Cache-Control标识的max-age/s-maxage），发现资源具有`Etage`声明，则向服务器请求时带上头`If-None-Match`（即响应头中`Etag`的值）。服务器收到请求后发现有头`If-None-Match` 则与被请求资源的相应校验串进行比对，决定返回200或304（**注意：服务器会优先验证If-None-Match**）。

emm，文字看起来文绉绉的，那就上图吧：

如图：![步骤图](/blog/img/cache/step_2.png)

**问**：咦？这个`Last-Modified`和`If-Modified-Since`貌似看起来够用了啊，为啥还要有`Etag`和`If-None-Match`啊？

**答**：

1. `Last-Modified`标注的最后修改只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，它将不能准确标注文件的修改时间
2. 如果某些文件会被定期生成，当有时内容并没有任何变化，但`Last-Modified`却改变了，导致文件没法使用缓存
3. 有可能存在服务器没有准确获取文件修改时间，或者与代理服务器时间不一致等情形

### 小结

当浏览器没命中强制缓存时，会和服务器协商是否使用缓存，这个就是**协商缓存**，协商缓存由响应头中的`Last-Modified`和`ETag`还有请求头中的`If-Modified-Since`和`If-None-Match`控制。命中协商缓存的资源，服务器会返回`304`

## CDN缓存

### 什么是CDN

CDN即Content Delivery network，内容分发网络。CDN可以理解为一个火车票的代售点，用户在浏览网站的时候，CDN会选择一个离用户最近的CDN边缘节点来响应用户的请求。例如，上海用户想要访问我[www.jayzangwill.cn](www.jayzangwill.cn)上的一个资源，这个时候，刚好我在上海有台CND服务器，用户的请求直接会被打到这台上海的CDN服务器上，而不会跑到北京深圳的服务器上去请求我的资源，这样就加快了服务器的相应时间。

### CDN的原理

在用户和服务器之间增加cache层，通过接管DNS,将用户的请求引导到cache上获得服务器的资源。

### CDN缓存策略

和浏览器缓存机制类似，CND也有一套类似的缓存策略，这套缓存策略会决定CDN服务器什么时候去更新自己的资源。

上文所说的`Cache-Control`的`max-age`头可以告知文件在浏览器的缓存时间，在`max-age`指定的时间内，浏览器会直接使用本地缓存，而不会请求服务器，CDN采取了类似的机制，你只要把CDN节点看成浏览器，源服务器看成浏览器需要请求的服务器即可，此时，源服务器的`max-age`头决定了资源在CDN节点本地缓存的时间，有一点差别的是，CDN规定了一个自定义协议，也就是上文说的`s-maxage`，若源站该`s-maxage`存在，会优先使用`s-maxage`作为CDN的缓存时间。

如下图为CDN的缓存机制示意图：

![CND缓存示意图](/blog/img/cache/cdn_step.png)

## 参考

[彻底理解浏览器缓存机制](https://www.cnblogs.com/shixiaomiao1122/p/7591556.html)
[浏览器缓存和CDN缓存基本介绍](https://blog.csdn.net/longaiyunlay/article/details/78390226)
[CDN缓存策略](https://www.cnblogs.com/quincyWang/p/6911664.html)
[设计一个无懈可击的浏览器缓存方案：关于思路，细节，ServiceWorker，以及HTTP/2](https://zhuanlan.zhihu.com/p/28113197)
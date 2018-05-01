---
title: 分享个人前端学习路线及面试经验
date: 2018-04-30 17:05:47
tags: [基础, javascript, html, css, 面试]
---

本人从大二上学期到现在学习前端已将近有3年时间了，最近利用毕业论文写完的一些空余时间写一下这篇文章，用于分享一些**个人**的前端学习经验，以及一些面试经验，不一定适合每个人，不喜勿喷，同时欢迎大家提出建议。

<!-- more-->

## 学习路线

总体的来说前端无非就是html、css、js只要把这三样的基础打好，什么都不是问题。他们的难易程度也是html < css < js，所以学习路线也是从html->css->js或者html&css->js。而我的学习路线是后者。

总的来说，在学完一个新东西的时候建议最好用新东西手撸个玩意出来。一定要动手实操！

### html&css

首先这部分的学习我是看了一本叫[Head First HTML&CSS](https://book.douban.com/subject/25752357/)的书，这本电子书可以到网上下载。这本书与平时看到的书有点不一样，这本书相当是**手把手教**你如何完成一个静态网站，虽然书有点老，但是拿来入门还是不错的。

看完这本书以后你的html&css基本上算是入门了，可以自己撸个个人主页或者博客什么的。看完书之后不动手撸个什么小玩意出来是不行的，光说不练等于白看。

这个时候如果感觉还需要加深html&css的话推荐[精通CSS（第2版）](https://book.douban.com/subject/4736167/)这本书，能力强的同学可以去学学[bootstrap](https://v3.bootcss.com/)，这是大名鼎鼎的前端html&css框架。会用了（其实也就是html标签上bootstrap事先加定义好的类名，没太大难度）以后就去看看bootstrap的源码。注意，这里所说的源码不是吧boosstrap.css从头看到尾，那会疯的。看的源码是你用到的那几个类名的源码。可以通过谷歌控制台去看，选中一个标签，然后在styles那看人家是怎么定义这个样式的。

![styles](/blog/img/summary/styles.png)

感觉差不多以后就得实际操作了，可以到[这里](http://ife.baidu.com/2016/task/all)的最下面的第一阶段任务去练练手。

![task](/blog/img/summary/task.png)

个人感觉这第一阶段任务还是挺适合新手去做的。先尽量不要使用任何框架，自己去手撸出来。最后不管撸没撸出来都要去看看人家是怎么完成这项任务的，学习一下人家怎么解决问题的。前提一定是自己得先去思考。

当然今年(2018年)的百度前端技术学院也是更加给力了，也有从0基础开始手把手教你如何完成一个页面。[传送门](http://ife.baidu.com/college/detail/id/5)

### git&github

这里插播一条git的学习指南，有的同学可能想要让自己的作品能让大家看到但是又没有自己的服务器的话怎么办？

那么这里就得借助[github](https://github.com/)和github pages了。首先你得明白怎么用git&github，[这里有份教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)，个人觉得还不错。

知道git&github怎么用以后，就利用github pages让别人看到我们的作品。

首先，利用git在本地创建gh-pages分支（注意：一定要是gh-pages分支），然后将各种资源用git推送到github对应仓库的gh-pages分支上（至于怎么操作不在这里详细说明，教程里已经说得很清楚了）

推送成功以后浏览器打开github用户名.github.io/仓库名/对应的html。举个例子，我在github上的用户名是[JayZangwillJayZangwill](https://github.com/JayZangwill)，然后我的仓库名是blog，然后这个仓库下的gh-pages分支有个index.html文件，这个时候访问的地址就是[https://jayzangwill.github.io/blog/index.html](https://jayzangwill.github.io/blog/index.html)。因为github pages默认以index.html文件作为入口，所以以上链接可简写为[https://jayzangwill.github.io/blog](https://jayzangwill.github.io/blog)。

git&github是现代公司团队合作的重要工具，不管你处于前端的哪个阶段，git&github都是必学的，是逃不掉的。

### js

如果百度前端技术学院的16年第一部分任务能够做差不多出来就可以学js了，如果感觉还是没有什么收获的话就得去面壁过了。

如果说html&css是餐前甜点的话js就是正餐了，js比html&css的难度高出不止一个档次，可能有些小伙伴看到js的时候就打算放弃了。这个时候建议不要慌，慢慢来节奏稍微慢一点。当时我刚看js的时候看的是看的是[Head First JavaScript](https://book.douban.com/subject/2372267/)，结果一点都看不懂，看到一半就放弃了。于是百度搜了前端js入门书籍，结果搜到了[JavaScript DOM编程艺术](https://book.douban.com/subject/6038371/)。这也是我比较推荐的一本入门书籍，如果能坚持看完，你的js就算入门了。

当然js的学习路径有很多，以上的路径不一定适合所有人，也可以去网上搜些视频，比如去[慕课网](https://www.imooc.com/course/list?c=javascript&)上去看看。

当感觉自己js学得差不多的时候可以找些练手的网站来加深对知识点的理解。个人推荐的网站有[2016百度前端技术学院第二部分js基础部分](http://ife.baidu.com/2016/task/all)，[Free Code Camp](https://freecodecamp.cn/home)。当遇到不认识的api或知识点时可以到[MDN](https://developer.mozilla.org/zh-CN/)上去查。

感觉自己js基本功差不多了以后，下一步就开始学习es6或者jQuery了。由于前端三大框架的出现，导致jQuery的地位受到动摇，如果没有万恶的ie的话，jQuery很快就要退出历史舞台了。不过就目前情况来看，jQuery在前端还是占有很大份额的，所以学习jQuery还是很有必要的。学习jQuery的话我只推荐一本[Head First jQuery](https://book.douban.com/subject/6688828/)，看完这本书即可。对于es6来说这是js在15年出的新规范、新语法，内容较多所以可以单独抽出一块来学习，这里我推荐阮一峰前辈的[es6入门](http://es6.ruanyifeng.com/)。

当es6看完以后就可以去学习前端的三大框架了，分别是Angular、Vue、React。当然也不是三个都要学，只需学好其中一个或者多个即可。就目前国内的情况来看Vue和React用的最多，而Vue相对React来说相对好学一点（ps:最近博主在学习React），所以我推荐的话Vue是比较好的一个切入点。另外附上[Vue的官方教程](https://cn.vuejs.org/v2/guide/)，个人认为官方教程已经写得很全面了，所以学Vue的话官方教程即可。React的话最近我也是在学习阶段，我这里就直接粘个[官方教程](https://doc.react-china.org/)吧。**注意**：学习它们之前一定要把es6看了，因为教程中用了一些es6的语法，为了防止文档看不懂，所以建议先把es6过了。

js是我在大学中花的时间比较多的地方，也是前端最重要的部分，想要学好它，光是按照我上面说的是不够的，更多的是需要你不断在实践中总结，发现问题和解决问题。有必要时可以去看别人是怎么写js代码的，或者看看框架的源码，这样学到的才更多。

### gulp&webpack

当你在前端的道路上走了一段距离的时候会发现想要一些东西来提高自己的开发效率，gulp&webpack就能够满足你。这些自动化工具能够自动编译代码、压缩静态资源、自动刷新浏览器等。

首先是gulp，gulp想要上手很容易，百度上搜一搜就有很多入门教程，这里推荐一篇[gulp入门教程](http://www.ydcss.com/archives/18)（我不会说我是从百度随便搜来的），虽然是我从百度上随便扒下来的但是足够入门了，毕竟gulp上手还是很容易的。这里还推荐一下我的[gulp配置](https://github.com/JayZangwill/myTools/tree/master/gulp)，能够满足大部分人的需求了。把gulpfile.js和package.json复制到本地，然后按照说明操作即可。

另外附上[gulp的插件搜索地址](https://gulpjs.com/plugins/)，搜出来的插件都会告诉你如何去配置，一般情况下只需把介绍上的配置复制上来即可。如图是[gulp-uglify](https://www.npmjs.com/package/gulp-uglify)的介绍。

![uglify](/blog/img/summary/uglify.png)

至于webpack，它上手可能会比较难，因为配置会比较繁琐（webpack4据说把一些东西内置了，可以不用配置文件就可以用）觉得比较难的同学可以先把webpack先放一放，前期的话gulp基本就可以满足需求。学习webpack的话感觉还是看[官方文档](https://doc.webpack-china.org/concepts/)比较好。同时，大家可以看看我的[vue-cli多页面模板](https://github.com/JayZangwill/vue-multipage)。

不过最近新出了个叫[parcel](https://parceljs.org/)的东西，号称极速零配置Web应用打包工具，大家可以了解一下。

这里有人可能会问为什么不学grunt？emm...这东西已经是过时的产物，有兴趣的同学可以去了解了解，不论如何gulp&webpack最终还是必须要掌握的。

### sass&less

sass和less都是css的预编译语言，它们不必两个都学，学了其中一个另外一个自然就会。我个人的话是学了sass，是在[慕课网](https://www.imooc.com/search/?words=sass)上学的，学习的话只需学习前两个即可。

![sass](/blog/img/summary/sass.png)

至于less的话，看[官方文档](https://less.bootcss.com/)吧，less或sass学完以后可以尝试把以前写过的css用sass或者less重写一遍用于加深对知识的理解吧。另外我的那个gulp配置默认是支持sass的编译的，如果用了我的那个gulp配置的同学且是学sass的就不用自己配置了，如果学less的同学得需要自己配置一下。

### nodejs

如果还有余力的同学可以去学学node，目前有些公司对会node的前端是加分项，这里推荐一本书[了不起的nodejs](https://book.douban.com/subject/25767596/)

### 一些学习网站推荐

至此，前端需要掌握的技能和学习路线已经说完了。这里推荐一些前端的进阶网站和一些论坛，让大家在前端路上走得更远。

1. [掘金](https://juejin.im/timeline) 在这里会有很多高质量的文章，我也经常在这逛的。
2. [知乎](https://www.zhihu.com/) 这里是大佬的世界
3. [stack overflow](https://stackoverflow.com/) 国外的一个超高质量技术论坛，你可以在这里提问，与国外大佬交流
4. [SegmentFault](https://segmentfault.com/) 国内版的stack overflow
5. [w3cplus](https://www.w3cplus.com/) 大漠老师创办的技术论坛
6. [Free Code Camp](https://freecodecamp.cn/home) 在线学习网站
7. [百度前端技术学院](http://ife.baidu.com/) 百度从15年到现在每年都会创办的技术学院
8. [前端月报](https://github.com/jsfront/month) 这里会有人每个月上传上一个月的一些高质量文章
9. [更多](https://www.zhihu.com/question/39503897)

### 一些书籍

1. [JavaScript高级程序设计（第3版）](https://book.douban.com/subject/10546125/) 红宝书，不解释，据说今年7月要出第四版，很期待啊。
2. [JavaScript权威指南](https://book.douban.com/subject/2228378/) 传说中的犀牛书
3. [你不知道的JavaScript](https://book.douban.com/subject/26351021/) 有上、中、下三卷
4. [JavaScript语言精粹](https://book.douban.com/subject/3590768/) 传说中的蝴蝶书
5. [JavaScript设计模式](https://book.douban.com/subject/3329540/)
6. [css揭秘](https://book.douban.com/subject/26745943/) 介绍了一些实用的css3新特性
7. [CSS权威指南（第三版）](https://book.douban.com/subject/2308234/)
8. [更多](https://www.zhihu.com/question/19809484)

## 面试知识点

接下来这些面试知识点，主要是针对实习生和应届生的，偏基础。有多年工作经验或者基础好的同学可以Ctrl+W了。

一般面试的时候面试官都是针对你简历上所写的来问，所以你会什么就写什么，不会的就别写上去了，以免面试的时候问到不会就尴尬了。

### 计算机基础

这里把计算机基础放在第一位是因为有很多人忽略计算机基础，前端是属于计算机行业的，不论什么方向，只要这个方向和计算机相关，就必须把计算机基础打好。同时，计算机基础也是面试官在面试实习生和应届生比较注重的一方面。

#### 数据结构和算法

数据结构在我面试印象中是考察得最多的计算机基础。

首先是各种排序算法，冒泡啊，选择啊，快速啊一些常用排序算法，我不建议大家去死记硬背这些算法，而是去记它的思想，这样代码自然就会出来。完之后有些面试官会问它们的时间复杂度，怎么得出的这个个时间复杂度。

然后到树的遍历，广度优先，前、中、后序遍历。

还有就是斐波那契数列，算法的时间复杂度为O(n)和O(n^2)的两种实现。

#### 计算机网络

tcp的三次握手和四次挥手过程，以及为什么要三次握手，四次挥手。

get和post的区别，建议看一下[这篇](http://www.techweb.com.cn/network/system/2016-10-11/2407736.shtml)文章。

http与https区别。

tcp与udp区别。

7层网络架构。

常见http状态码，301与302区别。304与200(form cache区别)，建议联系强制缓存与协商缓存来解释。

#### 操作系统

什么是进程，什么是线程以及他们的区别。

什么是死锁，怎么产生的，怎么解决。

### 前端面试知识点

#### html部分

这部分问的会比较少，因为实在没什么可以问的我所总结的知识点如下：

1. 什么是html语义化，为什么要语义化，有什么好处，怎么做。
2. html5新加的标签。
3. 有哪些行内元素，有哪些块元素。

#### css部分

1. `inline`与`block`与`inline-block`区别（即行内元素与块元素与行内块元素的区别），以及`inline`与`inline-block`元素间会有莫名的间距这个是怎么产生的，怎么解决。
2. position取值，以及他们之间的区别，而且是相对于谁定位的。父辈元素设置transform导致fixed定位失效情况。
3. 浮动造成的问题，以及如何清除浮动，清除浮动的原理，由此引出BFC的概念，什么是BFC，有什么特性。
4. 盒模型的组成，有哪些盒模型，以及如何互相转换。对于盒模型和定位的概念可以看我的[这篇](https://jayzangwill.github.io/blog/2016/12/15/CSS%20box%20model%20and%20location/)文章。
5. `animation`和`transition`区别。
6. `padding`和`margin`的单位设置为%时，这个%是相对于谁。
7. 水平垂直居中的多种实现方式。
8. css选择器的权重

#### js部分

之前也说了，js是前端最重要的部分，同时也是面试问的最多的，这里推荐我之前写的三篇文章：[几个让我印象深刻的面试题(一)](https://jayzangwill.github.io/blog/2017/03/01/Some%20of%20the%20interview%20questions%20that%20impressed%20me-1/)、[几个让我印象深刻的面试题(二)](https://jayzangwill.github.io/blog/2017/03/19/Some%20of%20the%20interview%20questions%20that%20impressed%20me-2/)、[四月北京面试之旅](https://jayzangwill.github.io/blog/2017/04/25/Interview%20in%20Beijing%20in%20April/)，同时js部分的知识点总结如下：

1. js的数据类型（一共7种）
2. this指向（注意严格模式下的this指向和箭头函数的this指向）
3. new的过程
4. 作用域与作用域链
5. 什么是闭包，闭包的作用及缺点（建议与作用域联系起来讲）
6. 原型与原型链，以及如何实现继承（多种实现方式，建议翻看红宝书继承部分或者看[我博客关于继承的讲解](https://jayzangwill.github.io/blog/2018/02/27/inherit/)，把es6的继承加上效果更佳）。
7. js的事件循环即Event Loop（macro task queue 和 micro task queue了解一下，宏任务包括哪些，微任务包括哪些，以及它们的执行顺序）
8. 前端跨域，什么情况会跨域，怎么解决，以及它们的优缺点，手写一下jsonp。
9. XSS攻击与CSRF攻击怎么攻击以及怎么防御。这里推荐一下[这篇文章](https://segmentfault.com/a/1190000006672214)。
10. 深拷贝与浅拷贝的区别，以及怎么实现（有的面试官会在这里问递归有什么缺点）。
11. ajax的过程，以及状态0~4分别发生了什么（有的面试官会让你手写ajax）。
12. es6新加的新特性。
13. Promise的三个状态，以及介绍一下async函数。

#### vue部分（因为我简历上写了会vue）

1. 双向数据绑定原理，这里推荐一下[这篇文章](https://juejin.im/post/5abdd6f6f265da23793c4458)
2. vue的生命周期，这里推荐一下大漠老师的[这篇文章](https://mp.weixin.qq.com/s/JsVx4DwhPwcP8jU0uYHr-g)还有掘金上的[这篇文章](https://juejin.im/post/5ad10800f265da23826e681e)
3. vue组件的数据传递（包括父->子、子->父，兄弟间，非兄弟）
4. 前端路由的实现，这里推荐[这篇文章](https://juejin.im/post/5ac61da66fb9a028c71eae1b)
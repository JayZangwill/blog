---
title: 移动端适配
date: 2019-04-02 15:55:55
tags: [移动端,适配]
---

## 前言

[原文](https://jayzangwill.github.io/blog/2019/04/02/flexible/) && [个人主页](https://www.jayzangwill.cn)

## 背景

随着移动端的普及，以及手机尺寸越来越多，这就衍生了众多的适配方案，以下挑一些常见的适配方案进行探讨。

本文默认读者已经对视口、物理像素、逻辑像素、css像素等移动端基本概念已经了解了。

## px + viewport适配

这种适配方案原理比较简单：实际上就是通过动态设置`meta`标签的viewport让css中的1px等于设备的1px。

js伪代码： 

```javascript
    const scale = 1 / devicePixelRatio

    head.appendChild(`<meta name='viewport' content='width=device-width, initial-scale=${scale}, maximum-scale=${scale}, minimum-scale=${scale}, user-scalable='no'>`)

    body.setAttribute('data-dpr', devicePixelRatio)
```

css伪代码
```css
[data-dpr='3'] {
    property: value * 3
    ...
}

[data-dpr='2'] {
    property: value * 2
    ...
}
```

大家可以用不同手机进入[这里](https://www.jayzangwill.cn/demo/flexible1.html)，或者用浏览器模拟。

**优点**：简单易于理解

**缺点**：不能涵盖所有dpr的机型，即使能，也会造成代码臃肿。

## rem布局

首先我们需要知道

1. `rem`的大小是基于页面根元素的`font-size`。
2. `rem`的本质是等比缩放。

例如：根元素的`font-size: 16px`则，`1rem`就等于`16px`；根元素的`font-size:32px`则，`1rem`就等于`36px`

可以到[这里](https://www.jayzangwill.cn/demo/flexible2.html)来意会一下`rem`的本质是缩放

这么一来，我们就可以利用这个特性动态设置根元素的`font-size`来达到我们想要的效果。

### 基于设计图的rem布局

通常我们拿到的设计图宽度的是750也就是基于iphone6/7/8的设计图，我们如果要想让1px像素等于设计图的1px该怎么做呢？

其实很简单，直接让根元素的`font-size: 0.5px`即可（因为是2倍图，1px等于2实际像素，所以为`0.5px`）。

那么问题来了：设计图一定会是750的，可是市面上机型那么多，屏幕不一定是750啊，怎么办？

![表情](/blog/img/common/biaoqing1.png)

前面我说过，**rem的本质是等比缩放**，让大于750或者小于750屏幕的手机等比缩放不就完事了？

js伪代码：

```javascript
html.fontSize = clientWidth / 750
```

你以为这就完事了？其实事情并没有那么简单。

![表情](/blog/img/common/yaoming.jpeg)

别忘了，平时我们开发都是在chrome下开发的。chrome并不支持`font-size`小于12的字体，那怎么办呢？

还能怎么办，为了方便计算，让`font-size`大于12呗，在以上基础上将结果放大100倍，然后写样式的时候再除以100：

![表情](/blog/img/common/baobao.jpg)

js伪代码：

```javascript
html.fontSize = clientWidth / 750 * 100
```

样式：

```css
.element {
    width: 0.1rem; /* 实际到6/7/8上就是20px */
}
```

### 基于屏幕百分比的rem布局

假设一个页面有100份，每一份的宽度用`x`表示，则`x=屏幕宽度(即clientWidth)/100`，如果`font-size: x`，那我们不就实现了`1rem === 1%`屏幕宽度了吗？

可是现实是残酷的，平时我们开发的时候写样式可不是按照屏幕宽度的百分之几来写的，而是根据设计给的标注来写的，而且设计通常给的设计图宽度是`750`的，那么如何转换呢？

其实可以换种思维：如果得到这个元素的宽度占页面宽度的百分之几不就完事了吗？

所以，`实际的rem = 元素标注长度 / 设计图宽度(这里是750) * 100`

当然，这个转换过程可以让[postcss](https://www.npmjs.com/package/px2rem)去帮我们转换。

### rem布局的优缺点

**优点:** 易于理解，且兼容性好，能够很好地解决适配问题。

**缺点：**

1. 需要用js额外设置字体大小,强制设置根元素的字体大小，剥夺了用户的自由
2. 在webview中，部分机型下用户设置系统默认字体大小会造成布局错乱

## 小程序的黑魔法rpx布局

做过小程序的都知道，小程序有个`wxss`，这玩意有个rpx的单位，[官方文档](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html)是这么介绍他的：`可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素`

再看到下面那个转换表时我瞬间就不淡定了：

![rpx](/blog/img/flexible/rpx.jpeg)

既是根据屏幕宽度自适应的又和上一章讲到的：基于设计图的rem布局的换算结果一样的，那它内部的实现原理其实和基于设计图的rem布局的原理差不多。

只不过小程序内部处理了一下，让rpx直接能够根据屏幕宽度自适应，而不是像rem那样依赖于根元素的`font-size`

## vw布局

上一章基于屏幕百分比的rem布局小节中提到的`x`代表1份屏幕的宽度，在css中刚好有个单位代表这个`x`，即vw，至于vw是啥这里就不多说了，都9012年了，竟然还不知道vw是什么，[传送门](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length)。

那么问题来了，平时我们拿到的设计图都是基于`px`标记的，怎么将`px`转为`vw`呢？

emmm，这里有个专门的插件专门做这件事[postcss-px-to-viewport](https://github.com/evrone/postcss-px-to-viewport)

其代码中有这么一句：

```javascript
toFixed((pixels / viewportSize * 100), opts.unitPrecision)
```

是不是和我们上面所说的`实际的rem = 元素标注长度 / 设计图宽度(这里是750) * 100`很像？

这个插件会解析我们的css文件，将`px`转换为`vw`,至于为什么要这么转化，上面已经说过了，这里就不再赘述。

`vw`完美地避开了`rem`的缺点，还继承了`rem`的优点，然而`vw`并不是完美的，由于`px`转`vw`是经过js计算的，多少都会优些精度损失，不过这是个小问题，不会丢失多少精度，除非你对精度要求特别高。

## 参考

[Rem布局的原理解析](https://yanhaijing.com/css/2017/09/29/principle-of-rem-layout/)
[rem, vw, 还是...? 各凭本事的移动端适配方案](https://juejin.im/post/5bc07ebf6fb9a05d026119a9)
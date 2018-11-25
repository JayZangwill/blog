---
title: line-height和vertical-align采坑记
date: 2018-11-24 19:56:20
tags: [基础,css]
---

由于在工作过程中经常遇到行内元素错位的问题，所以决定研究一下line-height和vertical-align，研究完后发现的确还有一些比较细节性的东西自己好不知道，这次打算和大家分享一下我自己的一些收获。

<!-- more -->

## 语法及规范

他们的语法和规范可点击一下链接，这里就不多说了。

### vertical-anign

[w3c规范](https://www.w3.org/TR/CSS2/visudet.html#propdef-vertical-align)

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/vertical-align)

下面列出一些规范中的重点：

1. baseline（默认值）：使元素的基线与父元素的基线对齐。HTML规范没有详细说明部分可替换元素的基线，如`<textarea>` ，这意味着这些元素使用此值的表现因浏览器而异。
2. sub：使元素的基线与父元素的下标基线对齐。
3. super: 使元素的基线与父元素的上标基线对齐。
4. text-top：使元素的顶部与父元素的字体顶部对齐。
5. text-bottom：使元素的底部与父元素的字体底部对齐。
6. middle：使元素的中部与父元素的基线加上父元素x-height的一半对齐。
7. length：使元素的基线对齐到父元素的基线之上的给定长度。**可以是负数**。
8. percentage：使元素的基线对齐到父元素的基线之上的给定百分比，**该百分比是line-height属性的百分比**（基友关系暴露）。**可以是负数**。
9. top：使元素及其后代元素的顶部与整行的顶部对齐。
10. bottom：使元素及其后代元素的底部与整行的底部对齐。

**注意：**没有基线的元素，使用外边距的下边缘替代，且vertical-align只对行内元素、表格单元格元素生效：**不能用它垂直对齐块级元素**。

#### 什么是可替换元素？

至于可替换元素的话可以看[这里](https://www.w3.org/TR/CSS21/conform.html#replaced-element)，英文好的人可以直接刚，英文不好的人直接看我的个人理解吧，可替换元素就是：这个元素显示的内容可以由标签熟悉决定，例如`img`标签的显示内容就是由`src`属性决定的，`src`改变，标签的显示内容也跟着变。

#### 基线在哪？

基线的问题可以看下面这张图：

![四线图](/blog/img/lineHeight/x_height.png)

这张图修改自[维基百科](https://en.wikipedia.org/wiki/X-height)

从图中可以发现一共有四根线，这四根线很像我们平时写英文时作业本上的那四根线，同时，图中标明了的找到`baseline`也就是基线的位置，同时还标明了`x-height`。(ps：css中有个以x高度为标准的单位即ex，1ex=1个小x的高度)

**but：**事情可没有这么简单，在[w3c规范](https://www.w3.org/TR/CSS2/visudet.html#propdef-vertical-align)的最下面有这么一句话：

The baseline of an 'inline-block' is the baseline of its last line box in the normal flow, unless it has either no in-flow line boxes or if its 'overflow' property has a computed value other than 'visible', in which case the baseline is the bottom margin edge.

翻译过来就是：`inline-block`的基线是正常流程中其最后一个线框的基线，除非它里面没有在流内（in-flow）或者其`overflow`属性值不为`visible`，在这种情况下，基线是`margin-bottom`边缘。

**提示：**所谓流内元素就是指没有浮动且`positions`的值为默认值或者`relative`以及非根元素。

蛤？不懂什么意思？来，看以下代码：

```css
    .wrap {
        display: inline-block;
        width: 100px;
        height: 100px;
        color: #fff;
        background: rgb(233, 78, 207);
    }

    .overflow {
        overflow: hidden;
    }
```

```html
    <div class="wrap"></div>
    <div class="wrap overflow">x</div>
    <div class="wrap">x</div>
```

结果:

![inline_box](/blog/img/lineHeight/inline_box.jpg)

首先根据上面那句话：“`inline-block`的基线是正常流程中其最后一个线框的基线”，他们的`baseline`是最后一个`.wrap`的`baseline`，即`字母x`的底部；然后根据第二句话：“除非它里面没有在流内（in-flow）或者其`overflow`属性值不为`visible`，在这种情况下，基线是`margin-bottom`边缘”；因为第一个`.wrap`内部没有其他`inline`元素和第二个的`overflow`值为`hidden`，所以他们两个的基线为`margin-bottom`的边缘，在这里因为没有设置`margin-bottom`，所以他们的基线在下面蓝色背景的边缘，这两个`.wrap`因为规范里的第二句话导致基线与第三个`.wrap`不一致，从而出现了如图所示的错位情况。


### line-height

[w3c规范](https://www.w3.org/TR/CSS2/visudet.html#propdef-line-height)

[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height)

下面列出一些规范中的重点：

1. normal 取决于用户端。桌面浏览器（包括Firefox）使用默认值，约为1.2，这取决于元素的 **font-family**
2. number 该属性的应用值是这个无单位数字`<number>`乘以该元素的字体大小。计算值与指定值相同。大多数情况下，这是设置line-height的推荐方法，不会在继承时产生不确定的结果。
3. length 指定`<length>`用于计算 line box 的高度。查看`<length>`获取可能的单位。以em为单位的值可能会产生不确定的结果。
4. percentage 与元素自身的字体大小有关。计算值是给定的百分比值乘以元素计算出的字体大小。百分比值可能会带来不确定的结果。

到这里可能有人会问： `number`和`percentage`不是一样吗？都是与自身字体大小有关。

关于这个，先上代码和效果：

```css
    .percentage-wrap {
        font-size: 30px;
        line-height: 100%;
    }

    .number-wrap {
        font-size: 30px;
        line-height: 1;
    }

    p {
        font-size: 20px;
    }
```

```html
    <div class="percentage-wrap">
        <p>x</p>
    </div>
    <div class="number-wrap">
        <p>x</p>
    </div>
```

效果如图：

![height](/blog/img/lineHeight/height.jpg)

可以发现，设置为百分比的`percentage-wrap`将行高遗传给了`p`，而`number-wrap`并没有；这就是二者的区别。

### 上面所说的line box是什么？

1. `content area` 是围绕着文字的一种box，高度由`font-size`和`font-family`决定。在谷歌控制台中，你用鼠标指向某个`inline`元素，蓝色区域就是所谓的`content are`。

2. `line box`其实就是用来包裹每一行文字的东西，一行一个`line box`，这东西我们平时看不见，摸不着。**line-box 的高度是由它所有子元素inline-box的最大高度计算得出的（即line-height或者height）**

这里借用一张图来说明`line box`（[图片出处](http://www.cnblogs.com/wfeicherish/p/8884903.html)）

![height](/blog/img/lineHeight/line_box.png)

图的代码：

```html
    <p>
        Good design will be better.
        <span class="a">Ba</span>
        <span class="b">Ba</span>
        <span class="c">Ba</span>
        We get to make a consequence.
    </p>
```

```css
    p  { font-size: 100px }
    .a { font-family: Helvetica }
    .b { font-family: Gruppo    }
    .c { font-family: Catamaran }
```

如图所示，这里有三行，共三个`line box`（ps：更多关于line box和line-height的信息可参阅[这里](http://www.cnblogs.com/wfeicherish/p/8884903.html)）。
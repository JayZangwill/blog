---
title: javscript的数据类型
date: 2016-11-07 09:24:10
tags: [javascript,基础]
---
# javscript数据类型

之前学到javascript的数据类型时遇到了一些困惑，因为之前和一些大神交流和自己百度+谷歌查到的还有在红宝书上看到的答案不一样，最近通过查阅了一些资料终于把自己的困惑解决了，现在来写一些笔记记录记录。

<!-- more-->

之前和一些大神交流了一下javascript的数据类型的问题，这些大神大多数都把js的数据类型分为：

* number

* string

* undefined

* function

* boolean

* object

当时我就把javascript的数据类型分为这几种了，但是后来我又在网上发现有其他的一种分类：

* number

* string

* undefined

* null

* boolean

* symbol (es6新增)

* object

可以发现，这两种分类的分歧在`function`和`null`上。

可以判断：前者是根据`typeof`返回的值来分数据类型的，根据我所查到的[资料](https://www.zhihu.com/question/24804474)说根据`typeof`来定义数据类型是不正确的，因为它只是一个运算符，它的返回值不能作为数据类型的依据。

[资料](https://www.zhihu.com/question/24804474)上面还说了`typeof null`返回的`object`是个历史性的错误，按理来说应该返回`null` 

**注意：**在javascript中

    console.log(null==undefined) //true
    console.lof(null===undefined) //false
    
在[ECMASscript标准中](http://www.ecma-international.org/ecma-262/6.0/#sec-ecmascript-data-types-and-values)也很详细地说了数据类型和它们的值，发现都是以第二种分类为准。

至于新加的符号类型可以[戳这里](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)
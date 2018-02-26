---
title: 几个让我印象深刻的面试题(一)
date: 2017-03-01 16:35:41
tags: [面试,基础,javascript]
---

## 前言

[原文地址](https://jayzangwill.github.io/blog/2017/03/01/Some%20of%20the%20interview%20questions%20that%20impressed%20me/)&&[我的博客](https://jayzangwill.github.io/blog/)
[知乎](https://zhuanlan.zhihu.com/p/25514220)&&[知乎专栏](https://zhuanlan.zhihu.com/jayzangwill)
[河南前端交流群官网](http://henanjs.org/)

时间不知不觉已经来到了大三的下学期，各大企业的春招也陆续的开始，自己也开始做些面试题，一来了解了解面试题一般会问什么，二来通过面试题可以发现自己还有哪方面不足赶紧弥补，以备战今年的春招。
通过最近的学习，我总结了一下一些我遇到的让我印象深刻的一些面试题，大家可以先看看题目自己想想答案，然后再看我的答案(**我的答案经供参考，如有更好的想法欢迎在下面评论区提出自己的意见**)。

<!-- more-->

## 正文

1.  给一个div设置它的宽度为`100px`，然后再设置它的`padding-top`为`20%`。问：现在这个div有多高？
2.  如下代码：


	function fn1(){
		for(var i=0;i<4;i++){
			var tc=setTimeout(function(i){
				console.log(i);
				clearTimeout(tc)
			},10,i);
		}
	}
	function fn2(){
		for(var i=0;i<4;i++){
			var tc=setInterval(function(i,tc){
				console.log(i);
				clearInterval(tc)
			},10,i,tc);
		}
	}
	fn1();
	fn2();

请问分别会输出什么？

3.  如下代码：


	var fn=function(a,b,c){
		return a+b+c;
	}

需要写一个函数，满足`curry(fn)(1)(2)(3) //6`

4.  使用原生JS实现：`(10).add(10).reduce(2).add(10) //28`，意思是10加上10减去2加上10等于28。

你可以自己思考答案后在参考我给的参考答案。

## 答案揭晓

### 第一个问题

这题主要考察了对**w3c**标准的了解。如果你亲自去浏览器去试的话会发现这个`div`的高为：`316.8`(**注意**：不同分辨率的电脑测试会有不同的效果，这里以我的电脑1600x900为参考)，其实到这里这题已经是解开了，但是可能还有些同学没明白这个`316.8`是如何计算得来的。别急，请听我细细道来。
![div的高](/blog/img/face/padding-top.png)
如果你搞不懂结果为何是这个的话可能会去查[w3school](http://www.w3school.com.cn/cssref/pr_padding.asp)，你可能会看到：
![w3school上的介绍](/blog/img/face/w3c.png)
但是可以这么说上面的所说的是错的，或者说，表述不准确。
例如一下情况：

	//css
	.inner{
			position: absolute;
			width: 100px;
			padding-top: 20%;
	}
	.mid{
		width: 200px;
	}
	.wrap{
		position: relative;
		width: 300px;
	}
	//html
	<div class="wrap">
		<div class="mid">
			<div class="inner"></div>
		</div>
	</div>
	
![加了定位后div的高](/blog/img/face/absolute-padding.png)
如果按照[w3school](http://www.w3school.com.cn/cssref/pr_padding.asp)说的，这个`inner`的高应该是`40px`，但是实际不是，而是`60px`，是以`wrap`的宽度计算的，由此可见，w3school的说法不成立。
那么，当`padding`设置为`%`时到底以谁为参考呢？
事到如今我也不给大家卖关子了，其实是以[包含块](http://www.ayqy.net/doc/css2-1/visudet.html#containing-block-details)为参考的。通俗点来说就是谁包含它，它就以谁为参考，在这里`inner`设置了`position:absolute`脱离了原来的文档流，就会去寻找它的祖先元素设置了`position:relative`的元素作为它的包含块。如果还不懂包含块是啥的同学建议仔细阅读我刚刚给的链接，同时还可以参考我在[segmentfault](https://segmentfault.com/q/1010000008362925)上的这个问题。

### 第二个问题

这题考察了对闭包和定时器另外还有js执行顺序的理解。
先来说说`fn1`，如果把`clearTimeout`去掉，相信大家一定很熟悉，都会说`10ms`延迟后会依次输出`0,1,2,3`。
但是，请注意这里加了个`clearTimeout`，如果你去控制台实验的话会发现只输出了`0,1,2`，那`3`呢？
先别急，请听我慢慢道来：
**请注意：**这个`tc`是定义在闭包外面的，也就是说`tc`并没有被闭包保存，所以这里的`tc`指的是最后一个循环留下来的`tc`，所以最后一个`3`被清除了，没有输出。

再来看看`fn2`，可以发现区别就是把`setTimeout`改为了`setInterval`,同时把定时器也传到了闭包里。
那么结果又会有什么不同呢？如果亲自去实验的同学就会发现输出`0,1,2,3,3,3...`。
什么鬼？为毛最后一个定时器没被删除。说实话，我在这里也想了很久，为何最后一个定时器没被删除。后来我为了调试方便把`i<4`改为了`i<2`并把触发时间改为3s，在浏览器中单步调试，发现3s后第一次触发回调函数执行的时候`tc`的值是`undefined`第二次触发的时候有值了。这个时候我顿悟，这和程序的执行顺序有关。我们知道js正常情况下是从上到下，从右到左执行的。
所以这里每次循环先设置定时器，然后把定时器的返回值赋值给`tc`。在第一次循环的时候`tc`并没有被赋值，所以是`undefined`，在第二次循环的时候，定时器其实清理的是上一个循环的定时器。所以导致每次循环都是清理上一次的定时器，而最后一次循环的定时器没被清理，导致一直输出`3`。

### 第三个问题

这题很明显，考察函数的柯里化，函数的柯里化还不懂是啥的童鞋可以看[这里](http://www.zhangxinxu.com/wordpress/2013/02/js-currying/)。
这题我的解题思路可能会有点愚笨，不过为了抛砖引玉，还是硬着头皮把我的方法写上来吧，欢迎大家在下面评论给意见。
按照题目的要求需要把`fn`作为第一个参数传进去，而且给出结果刚好是`fn`运算后给出的结果。
所以我的思路是要有一个函数将`fn`函数所需参数全部集中到一个数组上，集中完毕后调用`fn`函数，我的代码如下：

	var fn = function(a,b,c) {
		return a+b+c;
	}
			
	function curry(fn) {
		var arr = [],
		mySlice = arr.slice
		fnLen = fn.length;
			
		function curring() {
			arr = arr.concat(mySlice.call(arguments));
			if(arr.length < fnLen) {
				return curring;
			}
			return fn.apply(this, arr);
		}
		return curring
	}
	curry(fn)(1)(2)(3);//6

### 第四题

这题主要考察对函数原型的理解。
相信大多数同学看到题目可能会一脸懵逼。`(10)`是什么鬼？函数调用？如果是函数调用你的函数名咧？
但是如果你多看两眼`(10)`不就是普通的`10`嘛，就是这个`10`在调用它原型下的`add`函数，然后后面一串链式调用你懂的！
如果搞清楚这点的话这题就简单了，直接在`Number`的原型下加方法：

	Number.prototype.add=function(num){
		return this+num;
	}
	Number.prototype.reduce=function(num){
		return this-num;
	}

就是这么简单。

## 说在最后的话

祝各位和我一样奋斗在春招一线的同学早日拿到心怡的offer，另外，我的博客还会持续更新关于面试题的一些技术文章，欢迎大家关注。
[我的简历](http://www.jayzangwill.cn/resume.html)。

## 参考

[一个面试题衍生出来的问题推荐答案](https://segmentfault.com/q/1010000008362925)
[JS中的柯里化](http://www.zhangxinxu.com/wordpress/2013/02/js-currying/)
[Web前端面试题，求解答](https://www.zhihu.com/question/54822257)

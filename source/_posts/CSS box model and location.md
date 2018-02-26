---
title: css盒模型与定位
date: 2016-12-15 16:25:28
tags: [css,基础,盒模型,定位]
---

说到css的盒子模型和定位相信大家一定都听说过，因为它们是css中的基础，同时也是难点，这篇文章的作用在于基础知识的扫盲。

<!-- more-->

先来说说盒子模型吧

# 什么是盒子模型

简单地说每个html标签都是一个方块，然后这个方块又包着几个小方块。分别是：margin、border、padding、content。它们的关系是margin包着border包着padding包着content。就像盒子一层一层地包着一样，这就是我们所说的盒模型。
嗯，看上面的文字有点文绉绉的，我们直接上图吧。
打开谷歌浏览器，按下F12，然后把右边栏的滚动条拉到最下面你就会看到一个东西：

![盒模型](/blog/img/box/box-model.png)

这就很直观给我们展示了什么是盒子模型！

# 盒子有多大

我相信这个问题肯定会问倒很多人，这个问题是个非常经典的问题。我在百度上查都能查到有很多人写的博客上都在这方面有错误，所以，我觉得我有必要在这篇文章上讲清楚盒子到底有多大。
老规矩，先上代码

	.red{
		width:200px;
		height:200px;
		background-color:red;
	}
	
补全html代码就会在页面左上角出现这么个玩意儿：

![红色方块](/blog/img/box/width.png)

对应的盒子模型：

![红色方块的盒子模型](/blog/img/box/width-box.png)

很明显这个时候的盒子大小就是content的大小。来，我们继续往下走，我们给这个方块加上padding：

	.red{
		width:200px;
		height:200px;
		padding:10px;
		background-color:red;
	}

这时你就会发现这个方块比原来稍微胖了那么一点：

![加上padding的红色方块](/blog/img/box/padding.png)

对应的盒模型：

![加上padding的红色方块的盒子模型](/blog/img/box/padding-box.png)

这个时候将鼠标移到控制台上的这个元素你就会发现：

![加上padding的红色方块的长宽](/blog/img/box/padding-width.png)

下面写有盒子的长宽变成了`220x220`，很明显，padding是能够改变盒子的大小的，这时盒子大小就等于content+padding。

接下来，我们给盒子加上边框：

	.red{
		width:200px;
		height:200px;
		padding:10px;
		border:10px solid black;
		background-color:red;
	}
	
这个时候变成了下面这样：

![加上border的红色方块](/blog/img/box/border.png)

盒模型：

![加上border的红色方块的盒模型](/blog/img/box/border-box.png)

它的长宽：

![加上border的红色方块的长宽](/blog/img/box/border-width.png)

可以发现长宽变为了`240x240`，所以这时盒子大小就等于content+padding+border。

接下来讲margin。在给这个方块加margin之前为了方便观察我们加个类名为blue的div，并且加上样式：

	.blue{
		width:100px;
		height:100px;
		background-color:blue;
	}

效果图：

![加个蓝色方块](/blog/img/box/blue.png)

因为div是块级元素，所以新加的蓝色div自动跑到红色的下面。
接下来给红色方块加上margin-bottom：

	.red{
		width:200px;
		height:200px;
		padding:10px;
		margin-bottom:10px;
		border:10px solid black;
		background-color:red;
	}
	
效果图：

![加上margin](/blog/img/box/margin.png)

可以发现，盒子的底部产生了10px的空白。

对应的盒模型

![加上margin的红色方块的盒模型](/blog/img/box/margin-box.png)

方块的长宽

![加上margin的红色方块的长宽](/blog/img/box/margin-width.png)

很明显盒子的大小并没有变大，还是原来的`240x240`。

所以，最终盒子的大小为content+padding+border即内容的(width)+内边距的再加上边框，而不加上margin。

看到网上很多文章都把margin算进去了，如果按照他们所说的，上面盒子的大小应该是`240x250`，然而实际情况并不是。从这里可以看出，很多人对盒模型有误解。**把margin算进去的那是盒子占据的位置，而不是盒子的大小！**

其实盒模型一共分为两种，一种是上面讲的标准盒模型，还有一种是怪异盒模型，这两种盒模型的区别在于width/height。前者width/height指的是content区域的宽度和高度，后者width/height指的是content+padding+border。

在ie8+浏览器中使用哪个盒模型可以由`box-sizing`控制，默认值为`content-box`，即标准盒模型；如果将`box-sizing`设为`border-box`则用的是怪异盒模型。如果在ie6,7,8中DOCTYPE缺失会触发怪异模式。

我们可以把上面的红色方块的`box-sizing`设为`border-box`发现，无论我们怎么改border和padding盒子大小始终是定义的width和height：

![box-sizing](/blog/img/box/box-sizing.png)

# 定位

定位`(position)`有四个值：
1. static
2. relative
3. absolute
4. fixed

## static

一般如果我们不设置`position`的话它的默认值就是`static`，这个时候left、top、bottom、right是不起作用的
，现在有如下两个div，他们的关系是兄弟关系：

	.red{
		width:200px;
		height:200px;
		left:100px;
		top:100px;
		background-color:red;
	}
	.blue{
		width:100px;
		height:100px;
		background-color:blue;
	}

效果图：

![static](/blog/img/box/static.png)

可以发现，并没有什么变化。红色方块的`left`和`top`加不加都一样。

## relative

但是，现在我们给红色方块加个`position:relative`：

	.red{
		position:relative;
		width:200px;
		height:200px;
		left:100px;
		top:100px;
		background-color:red;
	}
	
嘿嘿，变这样了：

![relative](/blog/img/box/relative.png)

可以发现，红色方块跑到蓝色方块的右边了，左边缘和顶边缘都距离原来100px，但是蓝色方块还是在原来的地方不动，现在可以得出一个结论：**使用相对定位给元素加left/top/right/buttom元素会以原来的位置为基础加上这些值，即以原来的位置为基础定位，并且没有脱离文档流**

嗯，是不是看起来有点文绉绉的，那来个形象的比喻吧：

假如`position:static`是一个活人的话，并且拥有灵魂，这个时候我想给他加个`left:100px`想让他的灵魂出来，但是并没有效果。当这个人刚去世而且尸体还没火化的时候(相当于`position:relative`)，这时我加个`left:100px`灵魂就可以移动了，并且灵魂往尸体的右边移动了100px。因为尸体还没火化，所以这个人还是占一定的空间的。

## absolute

现在，我们把`position`改为`absolute`：

	.red{
		position:absolute;
		width:200px;
		height:200px;
		left:100px;
		top:100px;
		background-color:red;
	}
	
效果图：

![absolute](/blog/img/box/absolute1.png)

可以发现蓝色方块如我们所愿移动到了红色方块的上面，说明红色方块已经脱离文档流。虽然红色方块的位移和`relative`一样但是，红色方块位移的参考不再是原来的位置而是body只不过红色方块的位置刚好在body的最左上角，刚好碰巧位移一样，上面的这个例子可能看不出来，让我们来改改代码。

首先将html结构改成：

	<div class="red">
		<div class="blue"></div>
	</div>

然后红色方块的`absolute`改回`relative`

效果图：

![absolute](/blog/img/box/absolute2.png)
	
然后蓝色方块代码改为：

	.blue{
		position:absolute;
		top:100px;
		left:100px;
		width:100px;
		height:100px;
		background-color:blue;
	}


![absolute](/blog/img/box/absolute3.png)

可以发现在给蓝色方块添加`position:absolute`前，蓝色方块像我们想的那样在红色方块的左上角；当我们给蓝色方块添加`position:absolute`并且添加`left`和`top`时，蓝色方块就跑到了红色方块的右下角。

那么这次这个蓝色方块是以谁为参考进行位移的？刚刚你说的以body为参考又是什么情况？

好，我这里就给大家说清楚，当给一个元素设置`position:absolute`时，这个元素的位置就是以他父代元素`position`不为`static`的元素作为参考，如果他的父代元素都是`position:static`那就以body作为参考。刚刚红色方块的情况就是他父代元素没有`position`不为`static`的元素，所以只能以body作为参考。

嗯，讲的文绉绉的，我不喜欢。来，举个例子：

还是刚刚那个人与灵魂的例子，当设置`position:absolute`时，就相当于人死了，尸体已经火化了，只剩下灵魂和骨灰，所以是不占空间的(就是已经脱离文档流)。这个时候灵魂可以乱飘了，但是有个限制，只能相对于骨灰飘。

## fixed

现在让我们再来创建一个绿色方块：

	.green{
		position:fixed;
		top:150px;
		left:150px;
		width:100px;
		height:100px;
		background-color:green;
	}

效果图：

![fixed](/blog/img/box/fixed.png)

这个看起来貌似没有什么特别的，现在我们来给body加个`height:2000px`(这个高度随意，但是要使浏览器右边出现滚动条)，然后把浏览器的滚动条往下拉，一个神奇的事情发生了：绿色方块固定在我们定义的位置上屏幕上不动了！

![scroll](/blog/img/box/scroll.png)

这个fixed我们见得最多的就是网页中顽固的小广告，不管我们怎么拖拽滚动条，它总是固定在那，就是一个升级版的`absolute`。

用上面的例子来说就是人已经成仙了，可以不受限制地乱飘，而且不管你怎么拉，都拉不动他，他就在那不动了！

## z-index

说到定位，肯定少不了z-index。用上面的例子来说z-index就是灵魂飘的高度，设置得越大，自然就飘得越高，既然扯到了灵魂，z-index肯定是对活人(`static`)无效的了。

正常情况下(没有加z-index)，元素是按照后来居上原则进行堆叠。在这个例子上的html元素是这样的：

	<div class="red">
			<div class="blue"></div>
		</div>
	<div class="green"></div>

按照后来居上原则，红色方块最先被浏览器渲染到，所以在最下面，其次到蓝色，最后到绿色。

如果我们想让红色方块显示到前面我们可以给它加个`z-index:1`，结果：

![z-index](/blog/img/box/z-index1.png)

发现绿色方块“消失了”。其实绿色方块并没有消失，你可以将滚动条往下拉，或者看盒子模型：

![z-index](/blog/img/box/z-index-box.png)

可以发现它并没有消失，只是被盖在了红色方块下面。由于红色方块与蓝色方块是父子关系，红色的上来了，蓝色肯定的上来啊。

现在我有个想法，想让蓝色方块弄到红色方块下面该怎么办？很多人可能会想到简单啊，直接给蓝色方块加个`z-index:-1`不就得了吗？但是很可惜，没用。

要解释这个现象就得扯到**层叠上下文**的东西了。解释这东西又得花些篇幅来讲，具体的可以看[这里](http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/)。

这里我就一笔带过了，用最通俗易懂的例子来解释了，老爸的灵魂包着儿子的灵魂飘到了一楼(z-index:1)，但是儿子想下到-1楼(z-index:-1)这是不行的因为儿子的灵魂已经被老爸的灵魂包着了(当z-index为数值时，会产生层叠上下文)，如果儿子想下到一楼，得先不让老爸的灵魂包着儿子的灵魂(不设置红色方块的z-index或者设置为z-index:auto)，这样就能下到-1楼了这样蓝色方块就被红色方块挡住了。

但是不设置红色的`z-index`的话，绿色方块又出来作怪了，这个时候只能设置绿色方块`z-index:-1`，红色方块就没谁能挡得住了！

# 总结

1. css的盒模型由content(内容)、padding(内边距)、border(边框)、margin(外边距)组成。
2. 在w3c和模型中，设置的width/height是content的宽度/高度，在怪异模式中width/height设置的是content+padding+border宽度/高度。
3. 在w3c盒子模型中盒子的大小由content、padding、border决定，在在怪异模式中盒子大小由width和height决定。
4. 定位有四个值static(静止)、relative(相对)、absolute(绝对)、fixed(固定)。
5. left、top、right、bottom、z-index不能对static起作用。

有什么问题可以在下面留言哦，会及时给出答复的。

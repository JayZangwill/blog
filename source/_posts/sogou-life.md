---
title: sogou_life
date: 2018-03-19 22:02:27
tags: [心情,基础]
---

## 搜狗实习总结

转眼间，在搜狗实习已经快一年了，在这得确收获颇丰，所以得总结一下。

<!-- more -->

### 日常工作

1. 负责搜狗网页搜索结果页部分日常需求开发。
2. 负责搜索app的一些活动页开发。
3. 负责搜狗英文搜索和搜狗学术搜索全部日常需求开发及一些内部组件日常维护。
4. 搜狗其他周边需求的开发等。

### 工作中遇到的问题

因为平时工作中主要是html+css这一块的东西，所以遇到最多的问题就是浏览器兼容问题（ie7，ie8）与移动端适配问题。

下面列举一些工作中常见的一些兼容问题及解决办法：

1. pc下mac二倍屏图像显示模糊
```css
background-image: -webkit-image-set(url(imgUrl@1x) 1x,url(imgUrl@2x) 2x);
```

2. ie背景半透明效果（不是opacity哦，opacity会把前景也变透明）

```scss
$ieHexStr: ie-hex-str(rgba($color,$alpha));
filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#{$ieHexStr}', endColorstr='#{$ieHexStr}');
```

3. ie背景平铺

```css
filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(src=$url, sizingMethod='scale')
```

4. ie常用hack（在工作中通常解决一些间距、行高偏差）

```css
color:red;
color:green\0;ie8、9、10、11
color:#000\9;ie 8、9、10
color:orange\9\0;ie9、10
*color:yellow;ie7

/*ie以外浏览器红色 ie11绿色 ie9、10橙色 ie8黑色 ie7黄色*/
```

5. ie7下`display:inline-block`失效

解决办法：在ie7下`display:inline-block`只对默认是行内的元素生效，例如a、i、span

6. ie7下定位覆盖问题

有时候我们在定位的时候在其他浏览器下层叠顺序正常，但是在ie7下，无论给这个元素设置多高的z-index都没用，依然还会被另外一个元素覆盖，这是因为ie7下层叠顺序还得看父辈元素，这个时候，可以试试把父辈元素的z-index设高点（这个问题貌似在移动端也有，233）。

7. webkit(blink)内核下`fixed`定位失效。

看看`fixed`定位的父辈元素有没有设置`transform`属性，如果有的话就得去掉，因为在webkit(blink)内核下`transform`属性会导致`fixed`定位随着它定位，而不是随着窗口。

8. 在ios下给背景颜色设置`transparent`颜色不是变透明，而是变黑

解决办法：试试`rgba(255,255,255,0)`

9. ios下某个元素滚动不顺畅（松手时滚动太僵硬）

试试`-webkit-overflow-scrolling: touch;`

10. `sticky`定位失效

先确定浏览器版本是不是够新，够新的话检查父辈元素有没有有没有`overflow: hidden;`，有的话得删掉（sticky定位是个蛮新的定位值，不了解的同学这里有个[传送门](https://developer.mozilla.org/en-US/docs/Web/CSS/position)）

11. 微信小程序`textarea`组件自动聚焦有bug

解决办法：沟通换成`input`

12. 微信小程序雪碧图在真机上定位不准

解决办法：雪碧图换成`image`

13. 在做小程序时碰到一个需求：给`input`框加个**reset**功能，reset完以后要重新聚焦`input`，但是这个在真机上会有莫名的bug，感兴趣的同学可以试试。

解决办法：沟通把这个功能去掉

14. vm、vh、vmax、vmin在ios safair下宽度是大于视口的

解决办法：换单位或者用js兼容

15. 其他一些乱七八糟的问题

解决办法：利用调试工具快速定位问题（调试工具用的好，什么bug都不怕），如果无法解决（例如只有某种机型有bug），沟通解决（没有什么问题是沟通解决不了的，233）

### 收获

1. 项目实战经验，使得html+(s)css使用得更熟练（实习前基本都是用bootstrap的样式233）
2. 更注重html语义化
3. html+css+js基础加强，写出的代码质量更高
4. 熟悉了公司开发、合作流程
5. 将psd还原成页面更熟练，精度更高
6. 接触了pixi.js、bodmovin.js等一些动画库，更熟悉了canvas的一些原生操作
7. 考虑问题更全面
8. emm...

### 实习以前没见过的一些css属性

1. css多行截断

在实习前只知道怎么用css做单行截断，但是却不知到怎么用css做多行截断，css多行截断代码如下:
```
	overflow: hidden;
	display: -webkit-box;
	text-overflow: ellipsis;
	-webkit-line-clamp: 3; /*超出多少行开始截断*/
	-webkit-box-orient: vertical;
```
可以看到，前面加有-webkit-前缀，所以考虑到兼容性，目前只能用于移动端

2. -webkit-tap-highlight-color

这是用于改变a链接点击高亮的颜色

3. -webkit-overflow-scrolling: touch

在ios下设置了`overflow:scroll`的元素使其有惯性滚动效果。关于此属性的更多可以查看[这里](http://www.cnblogs.com/chris-oil/p/6164966.html)

4. filter: blur()

一个高斯模糊属性

5. filter: grayscale();

一个能使元素变灰的属性，一般用于图片上，使图片变灰。[更多filter的操作](https://developer.mozilla.org/en-US/docs/Web/CSS/filter)
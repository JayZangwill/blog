---
title: 几个让我印象深刻的面试题(二)
date: 2017-03-19 15:19:04
tags: [面试,基础,javascript]
---

## 前言

[原文地址](https://jayzangwill.github.io/blog/2017/03/19/Some%20of%20the%20interview%20questions%20that%20impressed%20me-2/)&&[我的博客](https://jayzangwill.github.io/blog/)
[知乎](https://zhuanlan.zhihu.com/p/25863288)&&[知乎专栏](https://zhuanlan.zhihu.com/jayzangwill)
[简书](http://link.zhihu.com/?target=http%3A//www.jianshu.com/p/65319f42a5ce)
[河南前端交流群官网](http://henanjs.org/)

上次写了一篇[几个让我印象深刻的面试题(一)](https://jayzangwill.github.io/blog/2017/03/01/Some%20of%20the%20interview%20questions%20that%20impressed%20me-1/)没看过的同学可以去看哦。
这次文章的题目来源：[这里有超过20家的前端面试题，你确定不点进来看看？](https://juejin.im/post/58c51b5c44d90400698da686)。
如果上面的问题在我这篇文章里没有提到的话，那就说明有些问题可以很容易查得到或者很简单或者我能力有限不能解答出来的。如果有的问题你不会而且我又没有提的那就认为就是我能力有限不能解答出来吧。嘿嘿嘿。开个玩笑，不过可以在下面留言哦！

<!-- more-->

## 正文

还是老规矩先给题目，然后在看我的答案，有什么意见可以在留言板提。

1. 请问a，b，c分别输出什么？

```javascript
function fun(n,o){
    console.log(o)
    return{
      fun:function(m){
         return fun(m,n);
      }
    };
}
var a = fun(0); a.fun(1); a.fun(2); a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1); c.fun(2); c.fun(3);
```

2. 用尽可能多的方法找出数组中重复出现过的元素

例如：[1，2，4，4，3，3，1，5，3]

输出：[1，3，4]

3. 给定一些文档（docs）、词（words），找出词在文档中全部存在的所有文档

```javascript
var docs = [{
        id: 1,
        words: ['hello',"world"]
    },
    {
        id: 2,
        words: ['hello',"kids"]
    },
    {
        id: 3,
        words: ['zzzz',"hello"]
    },
    {
        id: 4,
        words: ['world',"kids"]
    }
 ];
findDocList(docs,['hello']) //文档1，文档2，文档3
findDocList(docs,['hello','world']) //文档1
```

4. 下面代码会输出什么？

```javascript
var test = (function(a){
    this.a = a;
    return function(b){
        return this.a + b;
    }
    }(function(a,b){
        return a;
    }(1,2)));
console.log(test(1));
```

5. 不用循环，创建一个长度为 100 的数组，并且每个元素的值等于它的下标。
6. 一个整数，它的各位数字如果是左右对称的，则这个数字是对称数。那么请找出 1 至 10000 中所有的对称数
7. 以下代码输出结果是什么？
```javascript
var myObject = {
    foo: "bar",
    func: function(){
         var self = this;
         console.log('outer func : this.foo' + this.foo);
         console.log('outer func : self.foo' + self.foo);
         (function(){
             console.log('inner func : this.foo' + this.foo);
             console.log('inner func : self.foo' + self.foo);
         })();
    }
};
myObject.func();
```
8. 请写出以下正则表达式的详细规则说明
/^(0[1-9]\d\d?)?[1-9]\d{6}\d?$/
/^(1[89]|[2-9]\d|100)$/i
/^[\w-]+@[a-z0-9-]+({[a-z]{2,6}}){1,2}$/i
9. 请写出打乱数组方法
10. 写出element.getElementsByClassName 的实现方法
11. 请写出代码输出结果
```javascript
if(!("a" in window)){
    var a = 1;
}
alert(a);
```
12. 请写出代码输出结果
```javascript
var handle = function(a){
    var b = 3;
    var tmp = function(a){
        b = a + b;
        return tmp;
    }
    tmp.toString = function(){
        return b;
    }
    return tmp;
}
alert(handle(4)(5)(6));
```

13. javscript表达式"[]==''"的值是什么，为什么？
14. Js生成下面html，点击每个li的时候弹出1,2,3......
//li onclick事件都能弹出当前被点击的index=?
```html
<ul id="testUrl">
	<li>index=0</li>
	<li>index=1</li>
</ul>
```
15. map方法是ES5中新增的，要求为ES5以下的环境增加个map方法

## 答案揭晓

### 第一题

```javascript
function fun(n,o){
   console.log(o)
   return{
       fun:function(m){
           return fun(m,n);
       }
   };
}
var a = fun(0); a.fun(1); a.fun(2); a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1); c.fun(2); c.fun(3);
```

我们先来一步一步地看。首先是`a=fun(0)`因为只传了一个参数，`console`输出的是第二个参数的值，所以毫无疑问地输出`undefined`。

然后到`a.fun(1)`可以看出，这句话是调用前面`fun(0)`返回回来的一个对象里面的函数`fun`，这个`fun`又把`fun(m,n)`返回出去。这个时候**请注意**：这个对象里的`fun`在返回之前调用了一下`fun(m,n)`，所以`console`又会被执行，可以确定，它肯定不会输出传进去的1，因为1作为第一个参数传到`fun(m,n)`里，而`console`是输出第二个参数的。那么这次会输出啥呢？

好了，不给大家卖关子了，答案是0。可能有人会问了，纳尼？为毛是0，0是哪来的？

要想看明白我的解释，前提是你得清楚闭包。这里用到了闭包。我们知道，闭包有个功能就是外部作用域能通过闭包访问函数内部的变量。其实在运行`a=fun(0)`的时候，`return`出来的对象里的函数`fun`把传进来的这个0作为第二个参数传到`fun`里面并返回出来这时0得到了保存。所以当运行`a.fun(1)`的时候其实输出的是之前的0。后面的那两个调用也和这个的原理一样，最后都是输出0。

这里可能会有点绕，需要花点时间来看或者自行去调试。（我已经在尽力表达清楚了，如果还不懂的话就留言吧=.=）。

然后到`b`，如果前面搞懂了这里就不难了。`fun(0)`运行的时候会`return`一个对象出去，后面的一串链式调用都是在调用前面函数返回的对象里的`fun`，最终导致输出是`undefined 0 1 2`

最后到`c`，如果`b`都搞懂了，到这里基本就没什么难度了。分别会输出`undefined 0 1 1`。

如果还不懂的话建议单步调试一下，如果还是不懂可以在下面留言，我会尽最大能力给你解释。

### 第二题

用尽可能多的方法找出数组中重复出现过的元素
例如：[1，2，4，4，3，3，1，5，3]
输出：[1，3，4]

我的思路是，先创建一个数组。然后将传进来的数组进行排序。然后再利用`sort`方法遍历数组，因为它能一次取到两个数然后`a`和`b`比较如果相等而且`result`里面又没有重复的就把`a`推进去。

这是我的代码：

4.5日更新
----
感谢[@倔强的小瓶盖](https://www.zhihu.com/people/jue-qiang-de-xiao-ping-gai/answers)同学指出的问题

```javascript
function repeat(arr) {
    var result = [];
    arr.sort().reduce(function(a, b) {
        if(a === b && result.indexOf(a) === -1) {
            result.push(a);
        }
        return b;
    });
    return result;
}
```

```javascript
//之前问题代码
function repeat(arr){
    var result=[];
    arr.sort().sort(function(a,b){
        if(a===b&&result.indexOf(a)===-1){
            result.push(a);
        }
    });
    return result;
}
```

3.23日更新
----

感谢[@start-wrap](https://www.zhihu.com/people/start-wrap/answers)同学提供的方法：
```javascript
function repeat(arr){
	var result = [], map = {};
	arr.map(function(num){
	if(map[num] === 1) result.push(num);
		map[num] = (map[num] || 0) + 1;
	}); 
	return result;
}
```

值得一提的是`map[num] = (map[num] || 0) + 1`，这句代码的`(map[num] || 0)`如果`map[num]`存在，则`map[num]`+1反之则0+1，个人觉得用得很巧妙。

感谢[@早乙女瑞穂](https://www.zhihu.com/people/jerrywdlee/answers)提供的淫技巧：
```javascript
// es6
let array = [1, 1, 2, 3, 3, 3, 4, 4, 5];
Array.from(new Set(array.filter((x, i, self) => self.indexOf(x) !== i)));
// es5
var array = [1, 2, 4, 4, 3, 3, 1, 5, 3];
array.filter(function(x, i, self) {
    return self.indexOf(x) === i && self.lastIndexOf(x) !== i 
});
```

es6思路解说：

array.filter((x, i, self) => self.indexOf(x) !== i)
返回一个数组，该数组由arrary中重复的元素构成(返回N-1次)

new Set( [iterable] )
返回一个集合(重复元素在此被合并)

Array.from( [iterable] )
返回一个数组(将上一步的集合变为数组)

//es5思路解说：

使用`indexOf`和`lastIndexOf`正向判断和反向判断这个元素是不是同一个数(如果是同一个数，则两个方法返回的`i`是一样的)
### 第三题

给定一些文档（docs）、词（words），找出词在文档中全部存在的所有文档

我的思路是：把第二个参数的数组用`join`合成一个字符串，然后用`forEach`遍历，分别把文档里的`words`也用`join`合成一个字符串，利用`search`方法找每个文档里的`words`是否包含有`arrStr`。

这是我的代码：
```javascript
function findDocList(docs, arr) {
    let arrStr = arr.join(""),
    itemStr,
    result = [];
    docs.forEach(function(item) {
        itemStr = item.words.join("");
        if(itemStr.search(new RegExp(arrStr)) !== -1) {
        result.push("文档" + item.id);
    }
    });
    console.log(result);
}
findDocList(docs, ['hello']) //文档1，文档2，文档3
findDocList(docs, ['hello', 'world']) //文档1
```

### 第四题

下面代码会输出什么？
```javascript
var test = (function(a){
		this.a = a;
		return function(b){
		return this.a + b;
		}
}(function(a,b){
		return a;
}(1,2)));
console.log(test(1));
```

可以看到，这里有两个自执行函数。下面这个自执行函数执行完后向上面这个自执行函数传了个1所以`this.a=1`，这里的`this`指向`window`。然后这个自执行函数返回个函数给`test`变量。下面调用`test(1)`，这个1传进来后相当于`return 1+1`所以就输出2。

### 第五题

不用循环，创建一个长度为 100 的数组，并且每个元素的值等于它的下标。

如果了解[Object.keys](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)和[Array.form](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from)的话，这题基本上没啥难度。
答案：
```javascript
Object.keys(Array.from({length:100}))
```

哎！等下`Array.form`不是es6的吗，es5的怎么实现？
代码来了：

	Object.keys(Array.apply(null,{length:100}))
	
如果还不懂可以参考[这里](https://www.zhihu.com/question/41493194)的讲解。

### 第六题

一个整数，它的各位数字如果是左右对称的，则这个数字是对称数。那么请找出 1 至 10000 中所有的对称数

我的思路，先将数字转为字符串，然后利用数组的`map`方法遍历这个字符串，将字符串全部分开变为数组，然后调用数组的`reverse`方法，再将翻转后的数组`join`成字符串，最后对比翻转后的字符串和翻转前的字符串是否相等即可（方法有点愚笨，望大神指教）：
```javascript
function symmetric(){
    var i=1,
    str,
    newStr,
    result=[];
    for(;i<1000;i++){
        str=""+i;
        newStr=result.map.call(str,function(item){
            return item;
        }).reverse().join("");
        if(str===newStr){
            result.push(+str);
        }
    }
    return result;
}
```

### 第七题

以下代码输出什么？
```javascript
var myObject = {
    foo: "bar",
    func: function(){
    var self = this;
    console.log('outer func : this.foo' + this.foo);
    console.log('outer func : self.foo' + self.foo);
    (function(){
        console.log('inner func : this.foo' + this.foo);
        console.log('inner func : self.foo' + self.foo);
    })();
    }
};
myObject.func();
```

这题主要考察`this`指向，个人觉得难度不是太大，因为`this`已经被我完全承包啦(坏笑脸)。
这题的话只需考虑谁调用的函数`this`就指向谁。
函数开始执行`self=this`这里的`this`是指向`myObject`的，因为`myObject.func()`很明显是`myObject`在调用它嘛，所以头两句`console`输出的`foo`都是`bar`。
下面是一个自执行函数，要知道，自执行函数的`this`一般情况下都指向`window`这里也不例外，所以，第三个`console`输出的`foo`是`undefined`因为在`window`下`foo`没定义。第四个输出的是`self.foo`这个`self`就是上面定义的`self`即`myObject`所以，这里的`foo`为`bar`。

### 第八题

请写出以下正则表达式的详细规则说明
/^(0[1-9]\d\d?)?[1-9]\d{6}\d?$/
/^(1[89]|[2-9]\d|100)$/i
/^[\w-]+@[a-z0-9-]+({[a-z]{2,6}}){1,2}$/i

嘿嘿，正则也算我比较拿手的部分。我来一个一个解释吧，有些正则比较难用语言表达，大家意会意会吧。

第一个：首先`^`代表的是以它后面的一堆东西为开头`$`代表以它前面一堆东西为结尾，在这里的意思就是以`(0[1-9]\d\d?)?[1-9]\d{6}\d?`为开头和结尾的字符串。然后到第一个括号里的意思是匹配第一个字符串为0第二个字符串为1-9第三个字符串为0-9第四个字符串可有可无，有的话匹配1-9，然后这整个括号里面的内容可有可无。问好后面的意思是匹配第一个字符串是1-9然后后面6个字符串匹配0-9最后一个字符串可有可无，有的话匹配0-9。

所以整理整理就是：匹配以0为第一个，1-9为第二个，数字为第三个；第四个可有可无，有的话匹配数字；然后前面这一整坨可有可无。1-9为第五个(如果前面那一坨没有的话，则从第一个算起)然后后面6个都是数字最后一个数字可有可无的字符串，且以它为开头和结尾。

下面是例子：
022222222222  //true 
002222222222 //false 因为第二个数字是1-9
02222222222 //第一个括号最后一个数字**或**者最后面的数字省略
0222222222 //第一个括号最后一个数字**和**者最后面的数字省略
22222222  //第一个括号里的内容全部省略
02222222 //\d{6}没有满足。

第二个：匹配以1作为第一个，8或9作为第二个**又或者**以2-9为第一个，数字为第二个又或者匹配100的字符串，并以他们为开头和结尾，忽略大小写。

还是例子比较直观：

18 //true 匹配前面的1[89]
23 //true 匹配[2-9]\d
100 //true 匹配100
17 //false
230 //false

第三个：
匹配前面至少一个数字或字母或_或-再匹配@然后再匹配至少一个字母或数字或-然后到再匹配{字母2-6个}1-2个，的字符串，并以他们为开头和结尾忽略大小写。

这个用语言描述太难了，是我不会说话吗，上例子吧：

3@d{aw}{ad} //true
-@-{ddd}{fs} //true
3@3{dw}{ddd} //true
3@3{dw} //false {字母2-6个}少了一个即`({[a-z]{2,6}}){1,2}`后面的`{1,2}`没满足
@3{dw}{ddd} //false [\w-]+没满足
33{dw}{ddd} //false 没@
dsa@ffff{dw}{d} //false ({[a-z]{2,6}})不符合

### 第九题
请写出打乱数组方法

4.5日更新
----
[参考这里](https://www.h5jun.com/post/array-shuffle.html)

```javascript
// 之前的问题代码
function randomsort(a, b) {
    return Math.random()>.5 ? -1 : 1;
    //用Math.random()函数生成0~1之间的随机数与0.5比较，返回-1或1
}
var arr = [1, 2, 3, 4, 5];
arr.sort(randomsort);
```

### 第十题
写出element.getElementsByClassName 的实现方法
我的思路：先获取页面下的所有元素，然后用`split`将传进来的多个`class`分割成数组，然后利两层循环找出符合条件的元素（个人觉得这种方法效率实在低下，就当是抛砖引玉吧，欢迎留言）
代码：
```javascript
if(!document.getElementsByClassName) {
    document.getElementsByClassName = function(className) {
    var ele = [],
        tags = document.getElementsByTagName("*");
    className = className.split(/\s+/);
    for(var i = 0; i < tags.length; i++) {
        for(var j = 0; j < className.length; j++) {
        //如果这个元素上有这个class且没在ele里面(主要防止多个class加在一个元素上推进去两次的情况)
            if(tags[i].className === className[j] && ele.indexOf(tags[i]) === -1) {
                ele.push(tags[i]);
            }
        }
    }
    return ele;
    }
}
```
### 第十一题
请写出代码输出结果
```javascript
  if(!("a" in window)){
      var a = 1;
  }
  alert(a);
```

这题主要考察了变量的声明提升，任何变量(es5中)的声明都会提升到当前作用域的顶端。所以这里的代码其实为：
```javascript
  var a;
  if(!("a" in window)){
      a = 1;
  }
  alert(a);
```
所以，在if语句执行前`a`就已经在`window`中了，所以这里会`atert undefined`

### 第十二题
请写出代码输出结果
```javascript
var handle = function(a){
    var b = 3;
    var tmp = function(a){
        b = a + b;
        return tmp;
    }
    tmp.toString = function(){
        return b;
    }
    return tmp;
}
alert(handle(4)(5)(6));
```

我们来一步一步看：首先是`handle(4)`，到这里，程序开始运行，创建了一个`tmp`函数，同时把`tmp`函数的`toString`方法重写了，最后返回这个`tmp`函数。
**注意**：`tmp`里的`a`不是传进去的4，不要把`tmp`的`a`和`handle`的`a`搞混了，所以这里传的4啥也没干。

然后到第二步：`handle(4)(5)`，这里就是执行了`tmp`函数，这个时候`tmp`函数的`a`就是传进来的5，·`b`就是第一步函数执行的`b`即3(不懂为何是3的同学再去了解了解闭包吧)，最后这个`b`就等于8。

第三部重复第二步`8+6`，最后`b`为14，`javascript`引擎最后自动调用了`toString`返回`b`，所以结果是14。

### 第十三题
javscript表达式"[]==''"的值是什么，为什么？

这题考察对js`==`运算符的了解，我们知道`==`运算符如果两边值类型不一样会把它们转换为相同类型的值再来比较。这题左边是`object`类型，右边是`string`类型，所以会把左边的转化为`string`类型来比较，`[].toString()`就是`''`所以最后结果是`true`。

### 第十四题
Js生成下面html，点击每个li的时候弹出1,2,3......
//li onclick事件都能弹出当前被点击的index=?
```html
<ul id="testUrl">
    <li>index=0</li>
    <li>index=1</li>
</ul>
```

这题直接按照要求生成对应的html，再给`ul`绑定个事件，利用事件代理监听是谁被点了，然后输出它们的序号和对应的内容，没啥难度。我的代码：
```javascript
var ul=document.createElement("ul"),
    lis=[];
    ul.id="testUrl";
for(var i=0,li;i<2;i++){
    li=document.createElement("li");
    li.innerHTML="index="+i;
    ul.appendChild(li);
    lis.push(li);
}
ul.addEventListener("click",function(e){
    alert(lis.indexOf(e.target));
    alert(e.target.innerHTML)
});
document.body.appendChild(ul);
```
### 第十五题
map方法是ES5中新增的，要求为ES5以下的环境增加个map方法

个人认为只要对[map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map#Compatibility)方法够了解，自然就能封装出来了。嘿嘿，不喜勿喷。给的链接虽然也有一个实现`map`的方法，但是用到了es5的`for in`不符合题目，所以我的代码：
```javascript
if(!Array.prototype.map){
    Array.prototype.map=function(callback,context){
    var len=this.length,
        i=0,
        result=[];
    if (typeof callback !== "function") {
        throw new TypeError(callback + " is not a function");
    }
    context=context||window;
    for(;i<len;i++){
        this[i]!==undefined?result.push(callback.call(context,this[i],i,this)):result.push(this[i]);
    }
    return result;
    }
}
```
不过我的代码和标准的输出结果还是有点出入的。就是我不处理`undefined`和`null`，因为`this[i]!==undefined`，这两个值是会原样返回的。不过日常的一些需求还是能满足的。欢迎大家提建议哈。

终于打完了，这期就这么多题，希望能对大家有帮助，同时如果有不对的地方请及时指正，欢迎留言。

另外，欢迎大家来围观我封装的一个[ajax库 lightings](https://github.com/JayZangwill/lightings)。

## 参考
[JS随机打乱数组的方法小结](http://www.jb51.net/article/87094.htm)
[如何不使用loop循环，创建一个长度为100的数组，并且每个元素的值等于它的下标](https://www.zhihu.com/question/41493194)
[MDN map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map#Compatibility)
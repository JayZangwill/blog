---
title: 记一次我看红宝书对继承加深的理解
date: 2018-02-27 10:44:29
tags: [基础,javascript]
---

## 前言

[知乎](https://zhuanlan.zhihu.com/p/34123800)

趁着目前手头上没啥事，赶紧写篇文章，emm...。

红宝书不愧是红宝书，每看一次都会有新的收获，呃，废话不多说，直接进入正题吧。

此文章默认您已经对原型、原型链、对象实例、new的过程有了一定的理解，如不理解的请自行查阅相关资料。

另外，以下实例代码变量名、函数、变量值名我做了一定的修改，其大体思路和红宝书里的一样。

<!-- more-->

## 正文

在红宝书中，给出了以下继承方式：

1. 原型链实现继承
2. 借用构造函数
3. 组合继承
4. 原型式继承
5. 寄生式继承
6. 寄生组合式继承

接下来就一一对它们进行分析。

### 原型链实现继承

话不多说，先上代码：

```javascript
function Parent() {
    this.parent = 'parent';
}

Parent.prototype.getParent = function(){
    return this.parent;
}

function Son(){
    this.son = 'son';
}

Son.prototype = new Parent(); //这句代码是实现继承的核心
Son.prototype.getSon = function(){
    return this.son;
}
```

其思路就是让儿子的原型对象等于父亲new出来的实例；我们知道new出来的实例对象上会有其构造函数拥有的一系列属性。让儿子的原型等于这个实例自然就继承了父亲的一系列东西。

这个时候我们运行：
```javascript
var son = new Son();
son.getParent(); //parent
son.getSon(); //son
```
以上代码能正常运行的。

这个时候`son.constructor`是指向`Parent`的，因为`new Parent`出来的对象的`constructor`是指向`Parent`的，然后Son的`prototype`对象指向了这个new出来的对象，所以Son的`constructor`自然指向`Parent`。

这个时候我们来梳理一下它们之间的关系：

`son.__proto__ => Son.prototype => Parent new出来的实例对象.__proto__ => Parent.prototype`

因此当调用son.getParent()时实际上会经历三个搜索步骤：

1. 搜索son
2. 搜索son.__proto__（也就是new Parent()这个对象）
3. 搜索son.__proto__.__proto__（也就是new Parent().__proto__即Parent.prototype）

但是，使用原型链实现继承会有缺点，先上代码：
```javscript
function Parent(){
    this.child = [1]
}
function Son() {}
Son.prototype = new Parent();
var son1 = new Son();
son1.child.push(2);
var son2 = new Son();
son2.child.push(3);
son2.child //[1,2,3]
```
出现这种情况的原因是，所有实例都**共享**构造函数的prototype。

这个可以通过`son1.__proto__ === son2.__proto__ //true`和`son1.child === son2.child //true`来证明

为了解决这种问题，于是就出现了以下的方法：借用构造函数。

### 借用构造函数

惯例，先上代码：
```javascript
function Parent(){
    this.child = [1];
    this.getChild = function(){
        return this.child;
    }
}
function Son() {
    Parent.call(this);
}
var son1 = new Son();
son1.child.push(2);
son1.child //[1,2]
var son2 = new Son();
son2.child.push(3);
son2.child //[1,3]
```

可以看到，以上的引用问题解决了，但是，出现了一个与构造函数模式一样的问题，方法都在构造函数中定义，导致函数不能复用，所以，继续升级。

### 组合继承

```javascript
function Parent(){
    this.child = [1]
}
Parent.prototype.getChild = function(){
    return this.child;
}
function Son() {
    Parent.call(this);
    this.friend = 'Jay Zangwill';
}
Son.prototype = new Parent();
Son.prototype.getFriend = function(){
    return this.friend;
}
var son1 = new Son();
son1.child.push(2);
son1.getChild(); //[1,2]
var son2 = new Son();
son2.getChild(); //[1]
son2.getFriend(); // Jay Zangwill
```

这个方法解决了原型链继承的引用bug同时也解决了函数复用的问题，但是缺点也很明显，那就是调用了两次`Parent`函数，最终导致实例对象上有`child`属性原型链上也有这个属性。

在介绍升级方法前先介绍两种基础方法。

### 原型式继承

```javascript
var original = {
    child:[1],
    name:'Jay Zangwill'
}
function parent(obj){
    function F(){}
    F.prototype = obj
    return new F();
}
var son1 = parent(original);
son1.child.push(2);
son1.name = 'a';
var son2 = parent(original);
son2.child.push(3);
son2.child //[1,2,3];
son2.name = 'b';
son1.name //a
son2.name //b
```
这种继承方式得需要一个原始对象作为参考，这个方式的主要目的是与另一个对象保持类似。
在es5中新增了一个Object.create()方法来实现原型式继承，[传送门](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/create)。

### 寄生式继承

寄生式继承是原型式继承的增强版：
```javascript
function createObj(obj) {
    var clone = parent(obj);
    clone.getChild = function(){
        return this.child;
    }
}
```

这个方法与借用构造函数方法有个共同的缺点，就是方法没法复用。

### 寄生组合式继承

```javascript
function createObj(obj){
    function F(){};
    F.prototype = obj;
    return new F();
}
function inherirPrototype(parent,son) {
    var prototype = createObj(parent.prototype);
    prototype.constructor = son;
    son.prototype = prototype;   
}
function Parent(name){
    this.name = name;
    this.child = [1];
}
Parent.prototype.getChild= function(){
    return this.child;
}
function Son(name) {
    Parent.call(this,name);
    this.friend = [2];
}

inherirPrototype(Parent,Son);

Son.prototype.getFriend = function(){
    return this.friend;
}
var son1 = new Son('Jay');
son1.child.push(2);
son1.getChild() //[1,2];
son2=new Son('Zangwill');
son2.child.push(3);
son2.getChild() //[1,3]
son2.getFriend() //[2]
```

可以看到上面代码只调用了一次`Parent`没有生成多余的属性，同时还解决了使用原型方法时Son不能向Parent传值的问题，也没有属性引用的问题。

寄生组合式继承的大体思路是，通过借用构造函数模式来继承属性，通过原型链来继承方法。通过原型链继承方法无非就是想要得到父类的`prototype`对象，所以我们得想办法来拿到这个副本。而`createObj`函数就是干这事的。

可能有人会问：`var prototype = createObj(parent.prototype);`这句代码直接改成`var prototype = parent.prototype`不行吗？

答案是：肯定不行，要是这么改，要想在`Son.prototype`上加东西也会间接加到`Parent.prototype`上。

不过，在es5中，可以把`var prototype = createObj(parent.prototype);`换成`var prototype = Object.create(parent.prototype);`

它们之间的关系如下：

`实例(也就是son1和son2).__proto__ => Son.prototype => 中间函数的实例(也就是F).__proto__ => 中间函数.prototype => Parent.prototype`

### es6中的继承

在es6中，引入了`class`，这相当于是一个语法糖，这里我就直接上代码不展开说了，想要了解更多的话进入[传送门](http://es6.ruanyifeng.com/#docs/class)。

```javascript
class Parent {
    constructor(name) {
        this.name = name;
    }
    getName(){
        console.log(this.name);
    }
}
class Son extends Parent {
    constructor(name,age) {
        super(name);
        this.age = age;
    }
    getAge(){
        console.log(this.age);
    }
    let son = new Son('Jay', 18);
    son.getAge() //18
    son.getName(); //Jay
}
```

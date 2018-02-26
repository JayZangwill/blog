---
title: sass基础
date: 2016-08-09 19:27:28
tags: sass
---
# sass语法糖  

用了两天的时间看了一下sass的一些语法，对于熟悉css的我来说,看sass基本没遇到什么困难。  

为了避免忘记，现在来写些sass的一些笔记。

<!-- more-->

## sass安装  

因为sass是基于[Ruby的](http://rubyinstaller.org/downloads)，所以点开链接下载吧。  

安装过程中在选择路径的下面有三个选项![sass安装](http://img.mukewang.com/54f561190001531806350474.jpg "选第二个"),其它的我就不多说了。  

然后打开命令行工具输入：  


    gem install sass


稍等片刻，sass就安装好了。  

要更新的话在命令行工具输入：  


    gem update sass


就得了

## 开始编写sass

新建一个*.scss*文件，用你喜欢的编译器打开  

sass语法和css语法类似像下面：


    body{
    margin:0;
    padding:0;
    }
    

用[grunt](https://www.npmjs.com/package/grunt-contrib-sass)或者命令：


    sass "要编译的Sass文件路径"/"文件名".scss:"要输出CSS文件路径"/"文件名".css

编译出来的css代码：


    body{
    margin:0;
    padding:0;
    }


sass的`--watch "要编译的Sass文件路径"/"文件名".scss:"要输出CSS文件路径"/"文件名".css`命令也可以像[grunt-watch](https://www.npmjs.com/package/grunt-contrib-watch)一样监视文件的改动 

### 用变量编写sass

sass也可以声明变量，声明变量的方法`$+变量名`，例如：


    $variable=100px;
    body{
    width:$variable;
    }

编译出来就是：


    body{
    width:100px;
    }


如果要声明默认变量，在声明的变量后面加个` !default`即可。


    $variable=50px !default;
    body{
    width:$variable;
    }

编译出来就是：


    body{
    width:50px;
    }


### 嵌套
在sass中，可以使用选择器嵌套，属性嵌套，伪类嵌套,例如：


    nav{
        a{
            color:red;
            margin:{
                left:10px;
                top:100px;
            }
            &:hover
            {
                color:yellow;
            }
            &:visited
            {
                color:black;
            }
        header &{
            color:green;
        }
        }
    }


编译出来就是：


    nav a {
    color: red;
    margin-left: 10px;
    margin-top: 100px; 
    }

    nav a:hover {
    color: yellow; 
    }

    nav a:visited {
    color: black; 
    }

    header nav a {
    color: green; 
    }
    
    
  ### 混合宏
  
  混合宏是用来处理重复的样式，就像js中的有名函数一样，只要声明了，就可以无数次调用  
  
  
  声明：
  
  
      @mixin name{
            transform: scale(2);
            -webkit-transform: scale(2);
            -moz-transform:scale(2);
            -o-transform: scale(2);
            -ms-transform: scale(2);
      }


  调用：


      img{
        @include name;
      }


编译出来就是：


    img
    {
        transform: scale(2);
        -webkit-transform: scale(2);
        -moz-transform:scale(2);
        -o-transform: scale(2);
        -ms-transform: scale(2);
    }


当然，混合宏也可以传参数：


     @mixin name($scale){
        transform:$scale;
        -webkit-transform:$scale;
        -moz-transform:$scale;
        -o-transform:$scale;
        -ms-transform:$scale;
    }
    img{
        @include name(scale(2));
    }
    
    
编译出来就是：


    img {
    transform: scale(2);
    -webkit-transform: scale(2);
    -moz-transform: scale(2);
    -o-transform: scale(2);
    -ms-transform: scale(2); 
    }


这样，只要调用这个混合宏就不用像css那样写一堆兼容代码了，确实方便不少  

当然，混合宏也可以传多个参数，当参数过多时，可以用`...`代替，例如：


    @mixin box-shadow($shadows...){
    @if length($shadows) >= 1 {
        -webkit-box-shadow: $shadows;
        box-shadow: $shadows;
    } @else {
        $shadows: 0 0 2px rgba(#000,.25);
        -webkit-box-shadow: $shadow;
        box-shadow: $shadow;
            }
    }

    .box {
        @include box-shadow(0 0 1px rgba(#000,.5));
    }


编译出来就是：


    .box {
    -webkit-box-shadow: 0 0 1px rgba(0, 0, 0, 0.5);
    box-shadow: 0 0 1px rgba(0, 0, 0, 0.5); 
    }


### 继承

在css中有继承样式(子元素可以继承父元素的某些样式，例如font-family可以给子元素继承),在sass中可以使用`@extend`实现样式继承：


    .btn {
     border: 1px solid #ccc;
    padding: 6px 10px;
    font-size: 14px;
    }
    
    .btn-primary {
    background-color: #f36;
    color: #fff;
    @extend .btn;
    }


编译出来就是：


    .btn,.btn-primary {

    border: 1px solid #ccc;

    padding: 6px 10px;

    font-size: 14px; 
    }

    .btn-primary {

    background-color: #f36;

    color: #fff; 
    }


这样又减少了我们写重复的代码了！  

### 占位符%

Sass 中的占位符 `%` 功能是一个很强大，很实用的一个功能，因为 `%` 声明的代码，如果不被 `@extend` 调用的话，不会产生任何代码：


    %mt5 {
    margin-top: 5px;
    }
    %pt5{
    padding-top: 5px;
    }
    .btn
    {
        @extend %mt5;
    }


编译出来就是：


    .btn {
    margin-top: 5px; 
    }
    

### 插值`#{}`

使用 CSS 预处理器语言的一个主要原因是想使用 Sass 获得一个更好的结构体系。比如说你想写更干净的、高效的和面向对象的 CSS。Sass 中的插值就是重要的一部分，例如，最开始的那个混合宏还可以变为：


    @mixin name($transform,$scale){
    -webkit-#{$transform}:$scale;
    -moz-#{$transform}:$scale;
    -o-#{$transform}:$scale;
    -ms-#{$transform}:$scale;
    #{$transform}:$scale;
    }
    img{
        @include name(transform,scale(2));
    }


编译出来就是：


    img {
    -webkit-transform: scale(2);
    -moz-transform: scale(2);
    -o-transform: scale(2);
    -ms-transform: scale(2);
    transform: scale(2); 
    }


这样上面这个名为*name*的混合宏就可以变为更灵活，可以帮我们写各种css的兼容前缀。  

### 运算

在css中只有calc()函数可以进行数字运算。然而，在sass中可以直接使用算术运算符来对数据进行运算：


    .box {
    width: 20px + 8in;//1in=96px
    height: 8in - 20px;//当对变量进行运算时-前面需要有空格
    font-size: 10px * 2;
    background-position:(100px/2);
    }


编译出来就是：


    .box {
    width: 788px;
    height: 748px;
    font-size: 20px;
    background-position:50px;
    }

但对于不同单位进行计算时，会报错，例如：


    .box {
    width: 20px + 1em;
    }


编译时会报错：`Incompatible units: 'em' and 'px'.`

### 颜色运算

所有算数运算都支持颜色值，并且是分段运算的。也就是说，红、绿和蓝各颜色分段单独进行运算。如：


    p {
    color: #010203 + #040506;
    }


计算公式为 01 + 04 = 05、02 + 05 = 07 和 03 + 06 = 09   

编译出来就是：


    p {
    color: #050709;
    }


如果`#ffffff`再加的话编译出来就是`white`，同理`#000000`再减的话就是`black`  

`#000000`不论乘什么除什么编译出来都是`black`，`#ffffff`不论乘什么编译出来都是`white`

### 字符运算

在 Sass 中可以通过加法符号`+`来对字符串进行连接。例如：


    $content: "Hello" + "" + "Sass!";
    .box:before {
    content: " #{$content} ";
    }


编译出来就是：


    .box:before {
    content: " Hello Sass! ";
    }
    

注意，如果有引号的字符串被添加了一个没有引号的字符串 （也就是，带引号的字符串在 + 符号左侧）， 结果会是一个有引号的字符串。 同样的，如果一个没有引号的字符串被添加了一个有引号的字符串 （没有引号的字符串在 + 符号左侧）， 结果将是一个没有引号的字符串。 例如：


    p:before {
    content: "Foo " + Bar;
    font-family: sans- + "serif";
    }


编译出来就是：


    p:before {
    content: "Foo Bar";
    font-family: sans-serif; 
    }


至此sass基础第一篇文章就写完了，有什么意见的在下面留言吧。
---
title: angular学习笔记(1)
date: 2016-10-11 08:26:23
tags: [angular,基础]
---
# angular学习笔记(1)

由于之前学习了vue，所以学习angular感觉还蛮顺的，因为这两个框架有很多相似之处，例如：

    1. 输出都可以用双括号{{}}
    2. 都有双向数据绑定
    3. 指令也有很多相似的地方，这里就不一一列举了
    
<!-- more-->
    
下面准备开始，在开始之前先说明：下面代码的html文件的文档声明，还有头标签、文件的引入都省略了，直接从`ng-app`的内部开始写代码，js文件的模块声明如果没有特殊情况会默认使用`var myApp=angular.module("myApp",[]);`
    
# angular的模块化

在学习angular听到最多的就是模块化，任何一个功能都是模块。 

首先，要想用angular就得先引入angular.js文件，然后在*.html*文件中标签加入`ng-app`属性，(一般加在`html`标签或者`body`后面加，作为根部作用域)，告诉angular下面这块内容由angular管，同时自动启动angular，例如以下代码： 

    <html ng-app="myApp">
        <!--这里是angular的作用范围-->
    </html>
    
然后在自己的*.js*文件中声明一个变量，用于储存angular返回的模块： 

    var myApp=angular.module("myApp",[]);
    
括号中的`myApp`是标签中`ng-app`的属性值`"myApp"`，后面的中括号是模块的依赖项，第三个参数是一个回调函数，用来定义angular的一些服务。

如果在*html*文件里面如果没有写`ng-app`，这个时候就得在*js*文件里手动启动angular： 

    angular.element(document).ready(function(){
        angular.bootstrap(document,"myApp");
    });
    
同一个`ng-app`不能启动两次，如果启动两次会报错！

**注意：**一般一个应用中只能有一个`ng-app`，当然有少数情况也会有多个`ng-app`，但是`ng-app`不能嵌套在`ng-app`中，这个时候第二个以后的`ng-app`需要手动启动，不过一般不建议有多个`ng-app`。

# 控制器

一个应用中可以有多个控制器，一般一个控制器用于管理应用中的一个功能，例如：

HTML：

    <div ng-controller="controller1">
        <!--这里是一些指令-->
    </div>
    <div ng-controller="controller2">
        <!--这里是一些指令-->
    </div>

JS：

    myApp.controller("controller1",["$scope",function($scope){
        //这里是一些逻辑
    }]);
    myApp.controller("controller2",["$scope",function($scope){
        //这里是一些逻辑
    }]);

js中的`myApp`就是上面`angular.module("myApp",[])`返回的模块，调用`controller`方法创建一个控制器，第一个参数是HTML中`ng-controller`的属性值，第二个参数是一个数组，里面包含了控制器的一些依赖还有回调函数，`$scope`是angular的作用域，一般用于存储各种变量。

在angular中还有一个`$rootScope`，它是angular的根作用域链，类似于js中的全局作用域，定义在`$rootScrop`上的变量所有控制器都可以访问。

**注意：**

    1. 不要去复用controller，一个控制器一般只负责一块视图
    2. 不要再controller中操作DOM，这不是控制器的职责
    3. 不要再controller里面做数据格式化
    4. 不要再controller里面做数据操作
    5. 一般来说，controller不会互相调用，控制器之间的交互只会通过事件来进行

# 双向数据绑定

当年angular火有一部分原因是因为它实现了数据的双向绑定，这是其他框架都没有实现的。
所谓的双向数据绑定就是：数据模型里面的值变了，视图也会跟着变，反过来也是，像下面：

HTML：

    <div ng-controller="controller">
       <input type="text" ng-model="name">
       <div>{{name}}<div>
    </div>

JS：

    myApp.controller("controller",["$scope",function($scope){
        $scope.name="张三";
    }]);
    
运行以上代码在页面中就会看见有一个输入框里面的值是张三，下面有一段字也是张三。当我们改变输入框中的值会发现下面的那段字也会变，这就实现了数据的双向绑定。

# 指令

angular还有一个吸引人的地方就是指令系统。angular内置了很多的指令，同时，还可以让我们自定义指令。前面说到的`ng-app`和`ng-controller`就是指令。

## ng-bind

HTML：

    <div ng-bind="name"></div>
    
JS：

    myApp.controller("controller",["$scope",function($scope){
        $scope.name="张三";
    }]);

在页面中也会输出张三，和*双花括号*的作用一样，但是使用`ng-bind`的好处就是，当页面加载速度慢的时候页面中不会出现*双花括号*，保证了页面的美观性。

## ng-class

HTML：

    <div ng-class="my-style:true"></div>

`ng-class`一般用于控制class是否使用，如果`:`后面的表达式为`true`，就使用`:`前面的类名。

## ng-show和ng-hide

HTML：

    <div ng-show="true">我是显示的</div>
    <div ng-hide="true">我是隐藏的</div>
    
当`ng-show`后面的变量为`true`时，这个html元素就会显示，为`false`时就回隐藏，`ng-hide`刚好相反。

## ng-repeat

HTML：

    <div ng-controller="myController">
        <ul>
            <li ng-repeat="data in datas">{{data}}</li>
        </ul>
    </div>
    
JS：

    myApp.controller("myController",["$scope",function($scope){
        $scope.datas=["张三"，"李四","王五"];
    }]);

这个时候页面就会输出：

    1. 张三
    2. 李四
    3. 王五

`ng-repeat`就是用来遍历数据，并且把数据在页面中全部渲染出来，非常方便。

## ng-style

HTML：

    <div ng-style="{'color':'red','background-color':'green'}">字的颜色石红的，背景是绿的</div>

## ng-class-even和ng-class-odd

HTML：

    <div ng-controller="myController">
        <ul>
            <li ng-class-even="'even-style'" ng-class-odd="'odd-style'" ng-repeat="data in datas">{{data}}</li>
        </ul>
    </div>
    
JS：

    myApp.controller("myController",["$scope",function($scope){
        $scope.datas=["张三"，"李四","王五"];
    }]);

这样当`li`为奇数时，就会用`even-style`的class类，当`li`为偶数时就会用`odd-style`的class类。
    
## ng-click

HTML：

    <div ng-controller="myController">
        <button ng-click="changeStatus()">点我</button>
    </div>
    <p>{{status}}</p>
JS：

    myApp.controller("myController",["$scope",function($scope){
        $scope.status=false;
        $scope.changeStatus=function(){
            $scope.status=!$scope.status;
        }
    }]);

每点一次按钮，`changeStatus`函数就会执行一次，`stauts`的值就会改变一次

## ng-switch

HTML：

    <div ng-controller="myController">
        <button ng-click="changeStatus()">点我</button>
            <ol switch="status">
                <li ng-switch-when="true">
                    当值为true才会显示
                </li>
                <li ng-switch-when="false">
                    当值为false才会显示
                </li>
            </ol>
    </div>
    <p>{{status}}</p>
    
JS：

    myApp.controller("myController",["$scope",function($scope){
        $scope.status=false;
        $scope.changeStatus=function(){
            $scope.status=!$scope.status;
        }
    }]);
    
## ng-init

HTML：

    <div ng-init="[firstName='张',lastName='三']">
        {{firstName+lastName}}
    </div>
    
这是页面就会输出`"张三"`。`ng-init`用于初始化变量，不过初始化变量一般不这么做。

## ng-src

HTML：

    <img ng-src={{imgUrl}} alt="图片">

`ng-src`是用于解决 `src`属性用ng表达式bug的一个指令，如果地址中包含ng表达式，用`ng-src`比较好。

## 自定义指令

在angular内置的这些指令中肯定是不够用的，所以，angualr可以给我们自定义指令：

HTML：

    <hello-world>oldValue</hello-world>
    <div class="hello-world">oldValue</div>
    <!-- directive:hello-world -->
    <div>oldValue</div>
    <div hello-world>oldValue</div>

JS：

    myApp.directive("helloWorld",[function(){
        return{
            restrict: 'ECMA',
            template:"<div>newValue<span ng-transclude></span></div>",
            replace:true,
            transclude:true,
            link:function(){
                //在这里可以操作DOM、给元素绑定事件
            }
        }
    }]);

js部分有个`restrict`这个是匹配模式，用于匹配不同的属指令创建方式。

E代表匹配元素模式，也就是我们说的html标签`<hello-world>oldValue</hello-world>`，当需要创建带有自己的模板的指令时，使用这种方法。

C匹配class模式，就是上面的`<div class="hello-world">oldValue</div>`

M代表匹配注释模式，就是上面的`<!-- directive:hello-world --> <div>oldValue</div>`，这里需要注意的就是注释的开头和结尾要有个空格，不然angular是识别不出的。

A代表属性模式，即`<div hello-world>oldValue</div>`，当要为已有的HTML标签增加功能时，使用这种方法创建指令，这也是angular默认的匹配方式。

`template`是填充到标签里的内容

`template`里可以看到有个`ng-transclude`指令，这个指令是在下面的`replace`和`transclude`值为`true`时才有用。当`replace`为真时原来的*oldValue*会被替换成`template`里的内容，但是我们有时候希望保留*oldValue*，所以把`transclude`设为`true`，这个时候angular就知道要把*oldValue*保存下来。但是保留下来得有地方放，这个时候在`template`里面加个`ng-transclude`指令，就可以把*oldValue*放进去。

`template`可以换成`templateUrl`，后面接的是其他的html文件的地址。

`link`函数可以操作DOM、绑定事件、绑定作用域。

最终代码执行完后会得到(以`<hello-world>oldValue</hello-world>`的结果为例)：

    <div>
        newValue
        <span ng-transclude=""></span>
        <div>oldValue</div>
    </div>
    
### 自定义指令的controller和controllAs

还是上面的代码，将JS部分改为：

     var myApp = angular.module("myApp", []);
     myApp.directive("helloWorld", [function () {
         return {
             restrict: 'ECMA',
             template: "<div>newValue<span ng-transclude></span></div>",
             replace: true,
             transclude: true,
             controller:function($scope){
                 console.log($scope);
             },
             link: function () {
                 //在这里可以操作DOM、给元素绑定事件
             }
         }
    }]);
    myApp.controller('myController', ['$scope', function ($scope) {
        console.log($scope);
    }]);

这样在控制台输出的两个`$scope`实际上是同一个`$scope`。上面那个`controller`写在那里，其他指令可以通过`require`属性获得这个`controller`里面的东西，实现多个指令通过依赖注入进行通信。

### require

`require`可以将其他指令传递给自己，有三个用法：

    1. 通过驼峰法(directiveName)的命名指定了控制器应该带有哪一条指令，默认从同一个元素上的指令找
    2. ^directiveName，在父级查找
    3. ?directiveName，指令可选，找不到不要抛出异常
    
现在，上面的代码改成了：

     myApp.directive("helloWorld", [function () {
     return {
         restrict: 'ECMA',
         template: "<div>newValue<span ng-transclude></span><btn></btn></div>",
         replace: true,
         transclude: true,
         controller:function($scope){
             this.fun=function(){
                 alert("s");
             }
         },
         controllerAs:"test",
         link: function (scope,element,attr,test) {
             element.on("click",test.fun);
         }
     }
    }]);
    myApp.directive('btn', [function () {
        return {
            restrict: 'E',
            require:"^helloWorld",
            replace:true,
            template:"<button>点我</button>",
            link: function (scope, element, attrs,test) {
                element.on("click",test.fun);
            }
        };
    }]);

点击按钮会发现弹出两次a。那是因为在`helloWorld`指令里的`link`函数调用了一次`this.fun`下面的`btn`也调用了一次。如果点击文字就只会弹出一次。

这里的`require`用了`^`是因为`helloWorld`是`btn`的父级。

### scope

#### scope:true

在上面说了自定义指令里的`controller`和下面的`myController`是同一个控制器。但是当我在自定义指令里(`helloWorld`指令)加一句`scope:true`时，这两个控制器就有不同的作用域了：

    myApp.directive("helloWorld", [function () {
         return {
             restrict: 'ECMA',
             template: "<div>newValue<span ng-transclude></span><btn></btn></div>",
             replace: true,
             scope:true,
             transclude: true,
             controller:function($scope){
                 console.log($scope);
                 this.fun=function(){
                     alert("s");
                 }
             },
             controllerAs:"test"
         }
    }]);
    
这时，控制台输出的两个`scope`，展开来看会发现它们的*id*不一样，*id*小的那个是*id*大的父作用域，子作用域可以继承父作用域里的属性，父作用域读不到子作用域里的属性。

#### scope:{}

当`scope:{}`会创建一个独立作用域，有父元素，但是继承不到父元素的属性。

这个对象有三个参数：

    1 .&:把父作用域包装成一个函数，从而以函数的方式读写父作用域的属性
    2 .=:作用域的属性与父作用域的属性双向绑定
    3 .@：只能读取父作用域里的值
    
HTML：

    <div ng-controller="myController">
        <div hello-world a-data="data" b-data="data" data="{{data}}">oldValue</div>
        {{data}}
    </div>

JS：

     myApp.directive("helloWorld", [function () {
         return {
             restrict: 'ECMA',
             template: "<div>newValue<span ng-transclude></span><btn></btn></div>",
             replace: true,
             scope:{
                 a:"&aData",
                 b:"=bData",
                 data:"@"
             },
             transclude: true,
             controller:function($scope){
                 console.log($scope.a());
                 console.log($scope.b);
                 console.log($scope.data);
                 this.fun=function(){
                     $scope.$apply(function(){
                        $scope.b="aaaa"; 
                     });
                 }
             },
             controllerAs:"test"
         }
    }]);

在html中我加了三个属性分别为：`a-data`，`b-data`，`data`分别对应着JS中的`a`，`b`，`data`。第三个是简写形式，如果名字和属性名相同，则后面的名字可以不用写。

当按下按钮会发现页面上的*data*变为*aaaa*这就说明`b`是双向绑定的

**tips:**`@`不能读到对象；使用`@`时html标签里的属性值需要用双花括号括起来。

**小补充：**`priority`是用来设置指令的权值，也就是指令执行的顺序。`terminal`设置是否以当前设置的`priority`为界限，如果小于设置的`priority`就不执行。

就到这里，打完收工！
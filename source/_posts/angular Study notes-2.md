---
title: angular学习笔记(2)
date: 2016-10-14 8:05:20
tags: [angular,基础]
---

# angular学习笔记(2)

这里接着上一篇继续写笔记，废话不多说，直接进入正题……

# angular的服务

上篇文章说到，不要去试图复用`controller`。但是如果两个`controller`中有相同的功能该怎么办？

这个时候服务就起到作用了。angular内置有很多服务给我们使用，当然也可以给我们自定义服务。

<!-- more-->

## $http服务

$http服务是angular中使用得最频繁的服务，因为前台要不断和后台交互，所以$http使用得最频繁的一个服务。

HTML：

    <div ng-controller="myController">
        <ul>
            <li ng-repeat="data in datas">{{data.data}}</li>
        </ul>
    </div>
    
JS：

    myApp.controller("myController",["$scope","$http",function($scope,$http){
        $http({
            method:"GET",
            url:"dataUrl"
        }).success(function(data,status){
            $scope.datas=data;
        }).error(function(data,status){
            consloe.log("error")
        });
    }]);

模拟的数据：

    [
        {
            "data":"1"
        },
        {
            "data":"2"
        },
        {
            "data":"3"
        }
    ]

这样就会在页面输出：

    ·1
    ·2
    ·3

这个和jquery的`$.ajax`方法很相似,[这个是$http的API](https://docs.angularjs.org/api/ng/service/$http)，嘿嘿~。

## $filter服务

`$filte`也是angular中比较常用的服务，angular内置了9个filter分别是：currency(货币)，date(日期)，json(将数据转化为json)，lowercase(小写)，number(数字)，orderBy(排序)，uppercase(大写)，limitTo(限定字符串或者数组长度)。

filter可以嵌套使用(使用管道符号|分隔)。

    <p>{{time| date:"y-M-d H-m-s"}}</p>

例如上面这个会在页面输出年、月、日、时、分、秒。

## 自定义过滤器

这里用上面的http的例子：

HTML改为：

    <div ng-controller="myController">
        <ul>
            <li ng-repeat="data in datas | myFilter">{{data.data}}</li>
        </ul>
    </div>

JS中添加：

    myApp.filter("myFilter", function () {
            return function (obj) {
                var newObj = [];
                angular.forEach(obj, function (obj) {
                    if (obj.data.indexOf("1") !== -1) {
                        newObj.push(obj);
                    }
                });
                return newObj;
            }
        });

上面代码执行完以后会在页面打印出：

    ·1
    
当然，还有一种方法是在`$scope`上定义一个方法用作过滤器：

HTML：

        <div ng-controller="myController">
            <ul>
                <li ng-repeat="data in datas | filter: myFilter">{{data.data}}</li>
            </ul>
        </div>

JS：

    myApp.filter("myFilter", function () {
            return function (obj) {
                var newObj = [];
                angular.forEach(obj, function (obj) {
                    if (obj.data.indexOf("1") !== -1) {
                        newObj.push(obj);
                    }
                });
                return newObj;
            }
            $scope.myFilter=function(obj){
                if (obj.data.indexOf("1") !== -1) {
                        return true;
                    }
                return false;
            }
        });
        
这样也可以得到同样的结果。

当然，还有其他方法，在这我就不一一列举了。

## 自定义服务

在angular中有三种方法自定义服务，分别是`$provide.provider`，`factory`，`server`，这些方法的本质都是`provider`，只不过是后面两种方法将`$provide.provider`封装了一下，让我们使用起来更方便。

HTML：

    <div ng-controller="myController"></div>

JS:

    var myApp = angular.module("myApp", [], ["$provide",function ($provide) {
            $provide.provider("myServer", function () {
                this.$get = function () {
                    return function () {
                        console.log("myServer");
                    }
                }
            });
            
            $provide.factory("myServer2",function(){
               return function(){
                   console.log("myServer2");
               } 
            });
            
            $provide.service("myServer3",function(){
               return function(){
                   console.log("myServer3");
               } 
            });         
        }]);
    
    myApp.controller("myController",["$scope","myServer","myServer2","myServer3",function($scope,myServer,myServer2,myServer3){
        myServer();
        myServer2();
        myServer3();
    }]);
    
在上面，用三种方法创建了三个服务，运行以后在控制台会输出*myServer*、*myServer2*、*myServer3*。下面两个方法虽然写法一样，但是还是有些不同的，`factory`能返回所有数据类型的数据，而`service`只能返回对象或者应用类型的数据，例如：`service`不能直接返回字符串。

后面两种快捷方法可以不用写在回调函数里面，可以写成：

    myApp.factory("serverName",function(){
        return ...
    });
    myApp.service("serverName",function(){
        return ...
    });
    
## 路由

angular提供了一个路由的功能用于改变视图。只要链接改变，就可以改变相应的视图。如果需要使用得通过npm把*angular-router*从网上下下来。

使用的时候需要先注册：

    var myApp = angular.module("myApp", ['ngRoute']); 
    
完以后就可以使用了，首先，主html文件里面加上：

    <div ng-view></div>
    
*1.html*文件上(文件名可以随意取，文件内容也可以自己定)：

    <ul>
        <li ng-repeat="t in texts"></li>
    </ul>
    
*2.html*：

    <h1>我是标题</h1>
    <p ng-bind="text"></p>

JS：

    myApp.controller("helloCtrl",["$scope",function($scope){
            $scope.texts=["hello world","hello angular"];
        }]);
        
    myApp.controller("textCtrl",["$scope",function($scope){
            $scope.text="我是内容";
        }]);

    /*路由的配置*/
    myApp.config(function($routeProvider){
        $routerProvider.when("/hello",{
            templateUrl:"1.html",
            controller:"helloCtrl"
        }).when("/text",{
            templateUrl:"2.html",
            controller:"textCtrl"
        }).otherwise({
            redirectTo:"/hello"
        })
    });
    
从上面的js文件的配置可以看出，当地址栏输入`/hello`结尾的当前服务器地址时会将`1.html`里的内容放到主html文件的`<div ng-view></div>`内，当输入`/text`时就是`2.html`，其他的都默认为以`/hello`结尾，下面的控制器是必不可少的，用于控制相应的视图。

如果细心观察，会发现地址栏上的`/`前面会有个`#`，这个是为了防止浏览器向后台发送请求。

angular自带的路由有个缺陷，就是不能进行深层的嵌套路由。深层的嵌套路由就是路由里面还有路由。

不过，有个第三方的angular插件填补了这个缺陷，那就是[ui-router](https://ui-router.github.io/)，这个插件也得用npm下载：`npm i angular-ui-router`

使用时，也需要像angular-router一样那样注册：

    var myApp = angular.module("myApp", ['ui.router']);
    
主html改为：

    <div ui-view></div>

1.html：

    <h1>1 page</h1>
    <p>1</p>
    <div ui-view="home"></div>

home.html：

    <hr>
    <h1>home page</h1>
    <a ui-sref="link">link</a>
    
link.html：

    <h1>link page</h1>
    
JS：

    myApp.config(function($stateProvider,$urlRouterProvider){
        $urlRouterProvider.otherwise("/index");
        $stateProvider.state("index",{
            url:"/index",
            views:{
                "":{
                    templateUrl:"1.html"
                },
                "home@index":{
                    templateUrl:"home.html"
                }
            }
        }).state("link",{
            url:"/link",
            views:{
                "":{
                    templateUrl:"link.html"
                }
            }
        });
    });
    
从js的配置可以看出，打开页面默认是以`/index`结尾的视图。

打开页面可以看到*1.html*的内容(一个标题，一段文字)，*1.html*里面又嵌套了一个*home.html*的视图(一条线，一个标题，一个链接)，链接点开后会转到*link.html*的视图(一个标题)。

**tip**：`home@index`的`home`代表主页面嵌套视图里面的`ui-view`的值，a链接里的`ui-sref`的值代表`state`的第一个参数。
---
title: angular学习笔记(3)
date: 2016-10-24 09:51:09
tags: [angular,基础]
---

# angular表单验证

angular自带有一套功能强大的表单验证功能，另外还拓展了一些验证功能(number，url，email，reset)，还能给我们自定义验证方法，用起来特别方便。

首先，先得把bootstrap的样式引进来，这样就不用自己写样式了。

<!-- more-->

## 用户名的验证

直接上代码：

    <div class="form-group" ng-class="{'has-error':form.name.$dirty&&form.name.$invalid}">
                <label class="control-label">用户名</label>
                <input autocomplete="off" ng-pattern="/^[a-z]/i" name="name" type="name" ng-model="name" ng-required="true" ng-minlength="5" ng-maxlength="10" class="form-control" placeholder="邮箱">
                <div ng-show="form.name.$dirty&&form.name.$error.minlength" class="alert alert-danger help-block">
                    至少5位
                </div>
                <div ng-show="form.name.$dirty&&form.name.$error.maxlength" class="alert alert-danger help-block">
                    最多10位
                </div>
                <div ng-show="form.name.$dirty&&form.name.$error.pattern" class="alert alert-danger help-block">
                    以字母开头
                </div>
            </div>
            
从上往下看，可以先看到`ng-class`里面有个逻辑表达式，如果`has-error`后面的表达式成立就会用这个class。第一个表达式是`name`值为`form`的表单里的`name`值为`name`的输入框有改动返回的值就为真。第二个是这个输入框有不合法的话返回的值为真。

`ng-pattern`是指用正则验证，如果匹配则下面的`form.name.$error.pattern`就会返回`true`

## 密码和密码确认

    <div class="form-group" ng-class="{'has-error':form.password.$dirty&&form.password.$invalid}">
                <label class="control-label">密码</label>
                <input autocomplete="off" name="password" type="password" ng-model="password" ng-required="true" ng-minlength="5" ng-maxlength="10" class="form-control" placeholder="密码">
                <div ng-show="form.password.$dirty&&form.password.$error.minlength" class="alert alert-danger help-block">
                    至少5位
                </div>
                <div ng-show="form.password.$dirty&&form.password.$error.maxlength" class="alert alert-danger help-block">
                    最多10位
                </div>
            </div>
            <div class="form-group" ng-class="{'has-error':form.passwordConfirm.$dirty&&form.passwordConfirm.$dirty&&form.passwordConfirm.$invalid}">
                <label class="control-label">确认密码</label>
                <input autocomplete="off" name="passwordConfirm" type="password" ng-model="passwordConfirm" ng-required="true" class="form-control" placeholder="密码">
                <div ng-show="form.password.$dirty&&form.passwordConfirm.$dirty&&passwordConfirm!==password" class="alert alert-danger help-block">
                    两次不一样
                </div>
            </div>
            
密码的验证基本没什么特别的，最大长度和最小长度一眼能看出来什么意思。密码的确认那里，如果两个`ng-model`(指的是密码和确认密码的`ng-model`)不相等就返回`false`。

## 邮箱验证

    <div class="form-group" ng-class="{'has-error':form.email.$dirty&&form.email.$invalid}">
                <label class="control-label">邮箱</label>
                <input autocomplete="off" name="email" type="email" ng-model="email" ng-required="true" ng-minlength="5" ng-maxlength="30" class="form-control" placeholder="邮箱">
                <div ng-show="form.email.$dirty&&form.email.$error.minlength" class="alert alert-danger help-block">
                    至少5位
                </div>
                <div ng-show="form.email.$dirty&&form.email.$error.maxlength" class="alert alert-danger help-block">
                    最多30位
                </div>
                <div ng-show="form.email.$dirty&&form.email.$error.email" class="alert alert-danger help-block">
                    邮箱不对
                </div>
            </div>

angular里内置了邮箱的验证，将`input`的`type`改为`email`就行了，错误的返回值用`formName.inputName.$error.email`查看即可，不过个人觉得angular自带的*email*验证还不够完善，如果需要的话可以加个正则验证(`ng-pattern`)，也可以用下面讲到的自定义验证。

## 网址验证

    <div class="form-group" ng-class="{'has-error':form.blog.$dirty&&form.blog.$invalid}">
                <label class="control-label">博客</label>
                <input autocomplete="off" name="blog" type="url" ng-model="blog" ng-required="true" ng-minlength="5" ng-maxlength="30" class="form-control" placeholder="博客">
                <div ng-show="form.blog.$dirty&&form.blog.$error.minlength" class="alert alert-danger help-block">
                    至少5位
                </div>
                <div ng-show="form.blog.$dirty&&form.blog.$error.maxlength" class="alert alert-danger help-block">
                    最多30位
                </div>
                <div ng-show="form.blog.$dirty&&form.blog.$error.url" class="alert alert-danger help-block">
                    网址不对
                </div>
            </div>
            
angular里也自带有网址的验证，只需把`input`的`type`改为`url`即可，错误的返回值用`formName.inputName.$error.url`查看。

## 数字验证

    <div class="form-group" ng-class="{'has-error':form.age.$dirty&&form.age.$invalid}">
                <label class="control-label">年龄</label>
                <input autocomplete="off" name="age" min="0" max="99" type="number" ng-model="age" ng-required="true" class="form-control" placeholder="年龄">
                <div ng-show="form.age.$dirty&&form.age.$error.min" class="alert alert-danger help-block">
                    最小0
                </div>
                <div ng-show="form.age.$dirty&&form.age.$error.max" class="alert alert-danger help-block">
                    最大99
                </div>
                <div ng-show="form.age.$dirty&&form.age.$error.number" class="alert alert-danger help-block">
                    年龄不对
                </div>
            </div>
            
angular里面也对数字验证进行了拓展，和上面一样，把`type`改为`number`，错误的返回值用`formName.inputName.$error.number`查看。可以在标签上设置属性`min`和`max`来设置最小值和最大值，返回值分别用`formName.inputName.$error.min`，和`formName.inputName.$error.max`查看。

### checkbox

HTML：

    <div class="form-group">
            <label class="label-group">爱好</label>
            <label ng-repeat="hobby in hobbys" class="checkbox-inline">
                <input type="checkbox" ng-checked="checked===undefined ? false : checked.indexOf(hobby.id)!==-1" name="hobby" ng-click="toggleSelect(hobby.id)">{{hobby.name}}
            </label>
    </div>
    
JS：

    myApp.controller("myController",["$scope",function($scope){
        $scope.hobbys = [
        {
            id: 1,
            name: "游戏"
        },
        {
            id: 2,
            name: "游戏2"
        },
        {
            id: 3,
            name: "游戏3"
        }
    ];
    $scope.checked = [1, 2];
    $scope.toggleSelect = function (id) {
        var index = -1;
        if ($scope.checked === undefined) {
            $scope.checked = [];
        } else {
            index = $scope.checked.indexOf(id);
        }
        if (index === -1) {
            $scope.checked.push(id);
        } else {
            $scope.checked.splice(index, 1);
        }
    }
    }]);
    
这里的复选框我用了假数据(其实数据应该放在服务里面，我这里为了方便就放控制器里了)，然后用`ng-repeat`循环出来。然后用一个`checked`装已经选上爱好的*id*。下面的`toggleSelect`函数是判断`checked`数组里面是否有传进来的*id*如果没有，就加一个进去否则，删除一个。

## 城市三级关联

HTML：

    <div class="form-group">
                <label class="col-md-1 control-label">出生地</label>
                <div class="col-md-3">
                    <select class="form-control" ng-change="area=false" ng-model="city" ng-options="x.id as x.name for x in cities | filterCity:0"></select>
                </div>
                <div ng-show="city" class="col-md-3">
                    <select class="form-control" ng-model="area" ng-options="x.id as x.name for x in cities | filterCity:city"></select>
                </div>
                <div ng-show="area&&city" class="col-md-3">
                    <select class="form-control" ng-model="qu" ng-options="x.id as x.name for x in cities | filterCity:area"></select>
                </div>
    </div>

JS：

     myApp.controller("myController",["$scope",function($scope){
         $scope.cities = [
        {
            name: '上海',
            parent: 0,
            id: 1
            },
        {
            name: '上海市',
            parent: 1,
            id: 2
            },
        {
            name: '徐汇区',
            parent: 2,
            id: 8
            },
        {
            name: '长宁区',
            parent: 2,
            id: 3
            },
        {
            name: '北京',
            parent: 0,
            id: 4
            },
        {
            name: '北京市',
            parent: 4,
            id: 5
            },
        {
            name: '东城区',
            parent: 5,
            id: 6
            },
        {
            name: '丰台区',
            parent: 5,
            id: 7
            },
        {
            name: '浙江',
            parent: 0,
            id: 9
            },
        {
            name: '杭州',
            parent: 9,
            id: 100
            },
        {
            name: '宁波',
            parent: 9,
            id: 11
            },
        {
            name: '西湖区',
            parent: 100,
            id: 12
            },
        {
            name: '北仑区',
            parent: 11,
            id: 13
            }
        ];
     }]);
     myApp.filter("filterCity", function () {
     return function (city, par) {
         var filterData = [];
         angular.forEach(city, function (obj) {
             if (obj.parent === par) {
                 filterData.push(obj);
             }
         });
         return filterData;
     }
    });

像这样，就做好了城市3级关联。

在HTML中第一个下拉框有个`ng-change="area=false`，作用是：当这个下拉框的值改变时，第三个下拉框隐藏。

`ng-options`的作用和`ng-repeat`的作用类似，前者是循环创建*option*选项。可以看到它的值`x.id as x.name for x in cities`。`as`前面的值是*option*的`value`的值，后面的是用户可见的选项，`for`后面的表达式和`ng-repeat`的差不多。

下面的两个下拉框分别用上一个下拉框的`ng-model`值来过滤选项。

有些情况下，这三个单选框需要默认选中某个城市的某个区，这就要根据区来将父级城市选中，这时上面的JS部分增加几行代码变为：

    myApp.controller("myController", ["$scope", function ($scope) {
     $scope.cities = [
         {
             name: '上海',
             parent: 0,
             id: 1
            },
         {
             name: '上海市',
             parent: 1,
             id: 2
            },
         {
             name: '徐汇区',
             parent: 2,
             id: 8
            },
         {
             name: '长宁区',
             parent: 2,
             id: 3
            },
         {
             name: '北京',
             parent: 0,
             id: 4
            },
         {
             name: '北京市',
             parent: 4,
             id: 5
            },
         {
             name: '东城区',
             parent: 5,
             id: 6
            },
         {
             name: '丰台区',
             parent: 5,
             id: 7
            },
         {
             name: '浙江',
             parent: 0,
             id: 9
            },
         {
             name: '杭州',
             parent: 9,
             id: 100
            },
         {
             name: '宁波',
             parent: 9,
             id: 11
            },
         {
             name: '西湖区',
             parent: 100,
             id: 12
            },
         {
             name: '北仑区',
             parent: 11,
             id: 13
            }
        ];
     $scope.qu = 3;
     $scope.parentCity = function (id) {
         var parId;
         if ($scope.qu !== undefined) {
             angular.forEach($scope.cities, function (city) {
                 if (city.id === id) {
                     parId = city.parent;
                     return;
                 }
             });
         }
         return parId;
     }
         $scope.area = $scope.parentCity($scope.qu);
         $scope.city = $scope.parentCity($scope.area);
     }]);
     myApp.filter("filterCity", function () {
         return function (city, par) {
             var filterData = [];
             angular.forEach(city, function (obj) {
                 if (obj.parent === par) {
                     filterData.push(obj);
                 }
             });
             return filterData;
         }
     });
     
## 重置

angular的重置和默认的重置又有些不同。angular的重置需要把`model`里的数据也要重置，而默认的重置还做不到这点。

现在在原来的代码基础上将代码改为：

HTML：

            <div class="form-group">
                <button class="btn btn-info" type="reset" ng-click="reset()">重置</button>
            </div>

JS：

    myApp.controller("myController", ["$scope", function ($scope) {
     $scope.cities = [
         {
             name: '上海',
             parent: 0,
             id: 1
            },
         {
             name: '上海市',
             parent: 1,
             id: 2
            },
         {
             name: '徐汇区',
             parent: 2,
             id: 8
            },
         {
             name: '长宁区',
             parent: 2,
             id: 3
            },
         {
             name: '北京',
             parent: 0,
             id: 4
            },
         {
             name: '北京市',
             parent: 4,
             id: 5
            },
         {
             name: '东城区',
             parent: 5,
             id: 6
            },
         {
             name: '丰台区',
             parent: 5,
             id: 7
            },
         {
             name: '浙江',
             parent: 0,
             id: 9
            },
         {
             name: '杭州',
             parent: 9,
             id: 100
            },
         {
             name: '宁波',
             parent: 9,
             id: 11
            },
         {
             name: '西湖区',
             parent: 100,
             id: 12
            },
         {
             name: '北仑区',
             parent: 11,
             id: 13
            }
        ];
     $scope.init = function () {
         $scope.qu = 3;
         $scope.parentCity = function (id) {
             var parId;
             if ($scope.qu !== undefined) {
                 angular.forEach($scope.cities, function (city) {
                     if (city.id === id) {
                         parId = city.parent;
                         return;
                     }
                 });
             }
             return parId;
         }
         $scope.area = $scope.parentCity($scope.qu);
         $scope.city = $scope.parentCity($scope.area);
         $scope.copyQu = angular.copy($scope.qu);
     };
     $scope.reset = function () {
         $scope.qu = angular.copy($scope.copyQu);
         $scope.area = $scope.parentCity($scope.qu);
         $scope.city = $scope.parentCity($scope.area);
     };
     $scope.init();
     }]);
     myApp.filter("filterCity", function () {
         return function (city, par) {
             var filterData = [];
             angular.forEach(city, function (obj) {
                 if (obj.parent === par) {
                     filterData.push(obj);
                 }
             });
             return filterData;
         }
     });
     
在JS中，我把城市三级关联的初始化放到了一个`init`函数中，并在下面将需要的数据备份，当重置按钮被点下时运行`reset`函数，将三个下拉框的数据重置回来(在重置的时候也用到了copy是为了防止原始值和copy值互相引用，保证了数据的安全)，但是当用户在上面的用户名输入有错误时点击重置会发现下面的错误提示并没有消失，这是因为`$error`没被重置，这个时候，需要在重置函数里加上`$scope.formName.$setPristine();`就可以吧表单恢复为原始的状态，这样重置就完成了。
     
## 自定义验证

在前面说了，angular可以自定义验证规则。在用自定义验证规则之前得要知道有个`ngModel`，它可以更深层地处理数据的双向绑定，可以通过设置自定义指令的`require`为`ngModel`取到，并且将`link`函数的第四个参数设置为`ngModelController`

HTML：

    <div class="form-group" ng-class="{'has-error':form.even.$dirty&&form.even.$invalid}">
                <label class="control-label">偶数</label>
                <input autocomplete="off" name="even" type="number" ng-model="even" ng-required="true" class="form-control" placeholder="偶数" even>
                <div ng-show="form.even.$dirty&&form.even.$error.even" class="alert alert-danger help-block">
                    输入偶数
                </div>
    </div>

JS：

    myApp.directive('even', [function () {
         return {
             restrict: 'A',
             require:"ngModel",
             link: function (scope, element, attrs,ngModelController) {
                 console.log(ngModelController);
             }
         };
         }]);

可以看到在控制台会输出很多属性比如：

    1. $formatters 保存的是从modelValue向viewValue绑定过程中的处理函数
    2. $setViewValue 当view发生了某件事情时，从view向model绑定调用$setViewValue把viewValue保存下来
    3. $render 当模型发生变化时，应该怎么去更新视图，从model向view绑定，调用ctrl.$render方法，将viewValue渲染到页面上
    4. $setValidity 设置验证结果，第一个参数是验证错误的名字，第二个是true or false
    5. $viewValue 视图的值
    6. $modelValue 模型里的值
    7. $parsers保存从viewValue向modelValue绑定过程中的处理函数，它们将来会依次执行
    
这样就可以写自定义验证了，接下来，把JS代码改为：

    myApp.directive('even', [function () {
     return {
         restrict: 'A',
         require: "ngModel",
         link: function (scope, element, attrs, ngModelController) {
             console.log(ngModelController);
             ngModelController.$parsers.push(function (viewValue) {
                 if (viewValue % 2 === 0) {
                     ngModelController.$setValidity("even", true);
                     return viewValue;
                 } else {
                     ngModelController.$setValidity("even", false);
                 }
             });
         }
     };
     }]);


---
title: 初尝vue
date: 2016-08-14 10:14:47
tags: [vue,基础]
---

# 初尝vue

因为最近在做一个项目，感觉无从下手。因为老师给了一个漏洞百出的小破系统让我们自己研究做出个一模一样的出来(这里简单的吐槽一下)。在大神的建议下，开始学习vue，下面是我学习的一些笔记。

<!-- more-->

# Hellow World

vue的**hellow world**很简单：  

HTML：


    <div id="app">
        {{message}}
    </div>


js：


    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
                }
            });
    
    
结果：  


    Hello Vue.js!


代码通过[el](http://vuejs.org.cn/api/#el)获取到HTML的节点，向节点中的`{{message}}`插入[data](http://vuejs.org.cn/api/#data)里的`message`的值。    

**注意**HTML中的双括号里的名字和`data`里的名字一样

# 双向绑定

双向绑定就是用表单控件的`v-model`指令实现通过表单里的内容改变，来让`data`里的对象值改变，最后体现到页面上： 


在上面HTML代码的基础上加上： 


    <input type="text" v-model="message">


就可以实现数据的双向绑定

# 渲染列表

直接上代码：  

HTML： 


    <div id="app">
        <ul>
        <li v-for="todo in todos">
            {{ todo.text }}
        </li>
        </ul>
    </div>
    
    
JS： 


    new Vue({
        el: '#app',
        data: {
                todos: [
                    { text: 'Learn JavaScript' },
                    { text: 'Learn Vue.js' },
                    { text: 'Build Something Awesome' }
                        ]
              }
              });


结果：  


    · Learn JavaScript
    · Learn Vue.js
    · Build Something Awesome


这是用了vue的[v-for](http://vuejs.org.cn/api/#v-for)指令，用来循环渲染出数据。  

这个很像javascript里的`for-in`循环，`in`前面是自己定义的变量，`in`后面是要遍历的数组。这样，在这里通过`todo.text`就能取到`todos`数组里的东西，从而显示到页面上

# 事件监听

在vue中，可以使用指令[v-on](http://vuejs.org.cn/api/#v-on)监控对应的事件，可以缩写为`@`，例如： 


      <button v-on:click="dosomething">Reverse Message</button>


缩写为：


    <button @:click="dosomething">Reverse Message</button>


实例：  

HTML： 


    <div id="app">
        <p>{{ message }}</p>
        <button @:click="reverseMessage">Reverse Message</button>
    </div>


JS： 


    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        },
        methods: {
                reverseMessage: function () {
                this.message = this.message.split('').reverse().join('')
                }
            }
        });


当点击按钮时，Hellow Vue.js!的字母会变成!sj.euV olleH，再点击就会变回来。  

**注意**`methods`是用来放一些函数的，如果要想点击以后发生点什么，就把函数放到`methods`里。

# 综合

下列代码实现了向框里写一些东西后按回车就将框里的东西在下面显示出来，并且点击X按钮会删除对应的信息：  

HTML： 


    <div id="app">
        <input v-model="newTodo" v-on:keyup.enter="addTodo">
        <ul>
            <li v-for="todo in todos">
                <span>{{ todo.text }}</span>
                <button v-on:click="removeTodo($index)">X</button>
            </li>
        </ul>
    </div>


JS： 


    new Vue({
    el: '#app',
    data: {
        newTodo: '',
        todos: [
            { text: 'Add some todos' }
            ]
    },
    methods: {
        addTodo: function () {
        var text = this.newTodo.trim()
        if (text) {
            this.todos.push({ text: text })
            this.newTodo = ''
        }
    },
      removeTodo: function (index) {
      this.todos.splice(index, 1)
    }
    }
    });


在HTML代码中用到了`v-on:keyup.enter`，这句代码意思是监听回车按钮抬起事件。如果，回车按键抬起则执行后面的方法。下面的`$index`是取得当前列项的索引。  

`addTodo`方法是讲输入框输入的内容两边去掉空格，然后判断是否为空。如果不为空将这个字符串放到`todos`数组里，并且清空输入框。  

`removeTodo`方法是将用户选中的字符串删除。  

当然，在学习中还接触了一些*es6*语法例如：

* const 常量声明

* import 变量 from "路径" 和var something=require("路径")方法相似  

以前没见过的的一些东西：

* window.localStorage.getItem(key)，获取本地存储的一些数据。可以这么用： `JSON.parse(window.localStorage.getItem(STORAGE_KEY) || "[]")`意思是将获得的东西转化为JSON展示出来。

* window.localStorage.setItem(key,data)，向本地存储一些数据，第一个参数是存储的键值，第二个是数据。可以这么用：`window.localStorage.setItem(key, JSON.stringify(data))`意思是讲数据变为JSON格式存储
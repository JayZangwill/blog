---
title: vue-cli-multipage
date: 2018-03-06 11:40:12
tags: [vue,vue-cli]
---
# 基于vue-cli的多页面应用脚手架

## 前言

[原文](https://jayzangwill.github.io/blog/2018/03/06/vue-cli-multipage/#more)

[掘金](https://juejin.im/post/5a9e1716f265da237a4c85f9)

[知乎](https://zhuanlan.zhihu.com/p/34272390)&[知乎专栏](https://zhuanlan.zhihu.com/jayzangwill)

目前vue-cli生成的配置都是做多页面的，然而，我们有时也会有多页面的需求。
同时，之前借用的[这个](https://github.com/breezefeng/vue-cli-multipage)多页面例子貌似作者不再维护了，导致webpack升级到webpack2就无法使用了，所以我就参考这个例子自己弄了个多页面脚手架，会不定期维护的。

代码地址：https://github.com/JayZangwill/vue-multipage
有什么问题可以在issues上提，欢迎star

<!-- more -->

## 下载&使用

``` bash
git clone https://github.com/JayZangwill/vue-multipage
cd vue-multipage
npm i

//开发模式（运行完后要在浏览器输入http://localhost:8081/module/index）
npm run dev

//生产模式
npmrun build
```
## 目录结构
```
vue-multipage
  |---build
  |---config
  |---src
    |---assets   
    |---components  组件
      |---HelloWorld.vue
      |---other.vue
    |---module多页面模块
      |---index  
        |---index.html
        |---index.js
        |---App.vue
      |---other
        |---index.html
        |---other.js
        |---App.vue
```

## 说明

如需添加页面需要在**module**目录下新建文件夹，然后文件夹里必须包括`.hmtl，.js，.vue`文件作为入口文件。

运行`npm run dev`命令后，需要在浏览器输入`http://localhost:8081/module/+module下目录文件夹名/+文件夹名里的html文件`

## 已知bug

1. 目前公用css还无法分离
2. 开发模式需要手动输入url打开页面，不能直接打开。

同时也欢迎提代码pull来帮助我解决bug

## 参考

[vue-cli + webpack 多页面实例应用](http://www.cnblogs.com/fengyuqing/p/vue_cli_webpack.html)


---
title: hexo+github快速搭建个人博客
date: 2016-07-31 09:31:24
tags: hexo
---
# 开场白


昨天用*hexo*搭建了自己的一个个人博客，费了不少劲，网上的资料有的不全，有的是太老了现在用不了，废话不多说了，直奔主题吧。

<!-- more-->

## 正文


因为*hexo*是基于*node*的所以安装的时候非常方便


## 准备工作


 首先，我们来到[node官网](https://nodejs.org/en/)下载node(建议下载稳定版的)安装包，下载完后运行安装程序，然后一直点下一步就行了(安装的时候建议使用默认路径)。  
 
 然后，打开*cmd*或者*git bash*输入：
 
 
     node -v
     
 
 如果出现版本号说明安装成功。  
 
 接着，使用*node*自带的包管理工具*npm*安装*hexo*：
 
 
     输入npm install -g hexo(或者npm i -g hexo)
     
     
 安装全局的*hexo*  
 
 上面的工作做完后来到[github官网](https://github.com/)，没注册的先注册。然后点击**New repository**新建一个项目，名字为：`yourname.github.io` **注意：一个账号只能有一个yourname.github.io，如果有了就随便新建一个项目**。  
 
 新建完后，将仓库用
 
 
     git clone
 
 
 命令克隆到本地，然后进入到本地仓库，在这里打开命令行工具(也就是cmd或者git bash。**注意，路径是当前文件夹路径**)输入
 
 
     hexo init
 
 
 等一段时间后程序会帮你安装好*hexo*所需要的依赖。  
 
 安装完后，在命令行工具输入
 
 
     hexo generate(或者 hexo g)
     

等待一段时间后，输入


    hexo server(或者hexo s)
    
    
然后进入[localhost:4000](http://localhost:4000/)查看是否出现hexo的界面(有的人可能会发现样式没加载出来，不急，先往下看)   

如果出现界面的话，至此，所有准备工作基本完成，你离成功不远了！


## 全局配置


首先，得找到根目录的`_config.yml`文件，打开以后，把最上面的`Site`的博客基本信息填写完毕。  

**特别注意**：在冒号后面有空格，这个空格不能少  

然后来到下面的`URL`填写你博客的网址，例如我的：


    https:JayZangwill.github.io/blog
    
 

然后看到下面的这个`root`填写的是`io`后面的内容，我的是`/blog`(如果没有，那就是`/`**特别注意**：`root`后面不能为空)，之前说的样式没有加载很有可能是这个原因(如果还没加载出来，不急，再往下看)  

完之后，看到`_config.yml`文件的最下面，找到`deploy:`，另起一行输入：


    type: git
    repository: 你项目的ssh
    branch: gh-pages(如果博客地址是yourname.github.io，分支名就是master)


至此，全局配置就完成了。这个时候，你需要再在命令行工具输入：


    hexo generate(或者 hexo g)
    hexo server(或者hexo s)


如果标题变为你`_config.yml`文件最上面`title`输入的东西，那么文件就更新成功了！   

## 上传到*github*

在上传到github之前还需要安装一个东西，在命令行输入：


    npm install hexo-deployer-git --save
    
这样才能上传到*github*上，不然上传的话会报错  

之后在命令行工具输入：


    hexo deploy(或者hexo d)


即可上传到*github*上。   

传到*github*上以后那些样式和图片肯定都能加载上去了，如果还不能加载的在浏览器里按f12点击*network*选项卡看看路径怎么错了之后再调调根目录的`_config.yml`文件里的`root`吧！  


## 改变主题


我在改变主题这里花了蛮多的时间，一是因为没有找到符合胃口的主题，还有就是在主题更新这里走了很大弯路。直奔主题吧，伤心的会议不要再提！  

首先，我用的是[jacman](http://jacman.wuchong.me/2014/11/20/how-to-use-jacman)主题，界面还算看得过去。过几天有时间了在自己写个主题。给的网址上面说得很清楚主题是如何安装和配置的，我就不在赘述。  

我想说的是，上面说的更新：


    cd themes/jacman
    git pull origin master
    
    
是主题的源文件更新，不是我们博客的更新，我们博客更新是：



    hexo g


我在这就转了很大弯。。。我们在配置完主题的`_config.yml`文件后到根目录的命令行输入上面的命令更新博客后上传到*github*上就行了。  

**注意**：如果上传到*github*上有些链接点不开或者有些图片出不来，需要到本地仓库的public文件夹里的*index.html*或者*css*文件夹里的*style.css*里讲一些链接还有图片的链接进行微调,然后再更新上传即可


## 增加tags


当文章多了以后，我们需要一个标签来标记，想要增加标签，首先得安装一个东西：  


    npm i hexo-generator-tag --save


安装完后输入：  


    hexo new page "tags"   


然后会发现source里多出个tags的文件夹，这个文件夹里有个md文件，点开，在第二条横线下加个  


    type: "tags"


保存就好了。之后在根目录的命令行输入：  


    hexo new "文件名"  


你会发现在文章开头有个`tags: `，在后面填上标签即可。  

同理，新建分类也是一样的把`tags`换成`categories`就行了  

## 添加rss订阅


要想添加订阅功能首先得安装一个东西：


    npm install hexo-generator-feed


安装完以后再输入：


    hexo g
    
之后，你会发现在public里多了一个`atom.xml`文件，那就证明*rss*订阅功能已经添加了。  


至此，一个hexo博客就基本搭建完成了，如有疑问直接在下面留言，我有时间就会回复的。
---
title: 利用hexo搭建github静态博客
date: 2018-4-10 11:01:59
tags: mark1
---

Welcome to hexo, which is part of Dsetiny static blog, my blog address (http://zhjdeitiny.github.io/) want to here give you a different visual experience!<!--more-->

# 利用hexo搭建Github静态博客

<h3 style="color:deeppink">1、首先配置环境</h3>

#### <1>安装node（这是必须的）

```
作用是用来生成静态页面的
```

#### <2>安装Git（必须）
```
作用是把本地的hexo内容提交到github上面去，当然，安装Xcode自带git了
```

#### <3>申请Github账号（必须）
```
作用是用来做博客的远程创库、域名、服务器之类的，怎么与本地hexo建立连接等下讲
```

<h3 style="color:deeppink">2、正式安装Hexo</h3>

<h4 style="text-indent:2em"><1>安装好node和git后，首先创建一个文件夹，如blog等，用于存放hexo配置文件，之后在文件夹之后安装hexo即可</h4>

<h4 style="text-indent:2em"><2>执行如下命令安装hexo:sudo npm install-g hexo</h4>

<h4 style="text-indent:2em"><3>初始化hexo，执行如下命令:hexo init</h4>

<h4 style="text-indent:2em"><4>至此你的安装已经全部完成!blog就是你的博客根目录，所有的操作都在里面进行</h4>

<h4 style="text-indent:2em"><5>生成静态页面，执行命令: hexo g</h4>

<h4 style="text-indent:2em"><6>本地启动，执行命令:hexo s，然后获取地址，浏览器输入http://localhost:4000</h4>

<h4 style="text-indent:2em"><7>如果你的环境配置好的话，直接就可以看到你的博客，否则的话就是你的静态页面没有配置好</h4>

<h3 style="color:deeppink">3、配置GitHub</h3>

<h4 style="text-indent:2em"><1>建立与你用户名对应的仓库，仓库名必须为【your_user_name.github.io】(这是固定写法,不能改变，这点很重要！)</h4>

<h4 style="text-indent:2em"><2>然后建立关联，我的blog在本地/Users/leopard/blog，blog是我之前建的东西也全在这里面，有：_config.yml/node_modules/public/source       db.json/package.json/scaffolds/themes</h4>

<h4 style="text-indent:2em"><3>现在我们需要_config.yml文件，来建立关联，命令：vim _config.yml</h4>

<h4 style="text-indent:2em"><4>将最下面deploy改为type: git/repo:https://github.com/leopardpan/leopardpan.github.io.git    (注:提前设置SSH的话直接在github里面复制SSH即可)</h4>

<h4 style="text-indent:2em"><5>执行命令 npm install hexo-deployer-git --save</h4>

<h4 style="text-indent:2em"><6>执行命令 hexo d，在浏览器中输入http://leopardpan.github.io/   接下来你就可以看到自己的博客了</h4>

<h3 style="color: deeppink">一些常用的命令</h3>

<h4 style="text-indent:2em">hexo new"postName" #新建文章</h4>

<h4 style="text-indent:2em">hexo new page"pageName" #新建页面</h4>

<h4 style="text-indent:2em">hexo generate #生成静态页面至public目录</h4>

<h4 style="text-indent:2em">hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）</h4>

<h4 style="text-indent:2em">hexo deploy #将.deploy目录部署到GitHub</h4>

<h4 style="text-indent:2em">hexo help # 查看帮助</h4>

<h4 style="text-indent:2em">hexo version #查看Hexo的版本</h4>




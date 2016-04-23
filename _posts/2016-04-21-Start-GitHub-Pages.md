---
layout:     post
title:      "使用GitHub Pages创建静态Blog"
subtitle:   "Personal Blog Based on GitHub Pages"
date:       2016-04-21 12:00:00
author:     "dytan"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
    - GitHub
    - Blog
    - Markdown
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

## 一 说明

个人博客的构建有多种方式，最便捷莫过于选择一个免费空间、博客网站来写，国内CSDN、博客园、51CTO乃至新浪等网站是开通技术博客最为常用的
站点，这种方式十分便利但有不少限制；第二种方式是自己购买域名和空间，配置服务，构建独立博客完全自主控制，只是管理起来过于麻烦。

GitHub Pages是全球当前最大代码托管网站[GitHub](https://www.github.com)推出的一项服务，可用于上搭建blog，这种方式构建博客我们可以
拥有绝对管理权，又享受GitHub的便利，只要向主机提交commit，就能发布新文章，GitHub提供无限流量，访问速度十分理想。

近日开始使用[GitHub Pages](https://pages.github.com/)搭建Blog，虽然对GitHub的熟悉程度不够，Web前后端的开发、框架也是外行，但参考网络资料利用喜欢的模板，从零开始
到简单可用的博客仍然十分简单便捷，很快便能成功，现记录一些构建过程。

## 二 GitHub Pages简介

GitHub如今已是声名在外，涉及编程的人对其必有了解，可以简单理解为一个具有版本管理功能的代码仓库，每个项目都有一个主页，列出项目的源码。

GitHub Pages是GitHub的一项功能，直接建立于GitHub repository上，一个GitHub帐号可以建立一个名为_username.github.io_(其中username为帐号用户名，".io"可替换为".com")的repository,
在此创建配置博客内容后，GitHub会自动为这个repository生成网站，通过地址_http://username.github.io 即可访问生成的博客，配合Git使用十分便利，每次更改或新建Blog
文章后，push到该GitHub repository中，即可更新发布。

GitHub Pages使用jekyll技术，一个普遍的实现方式是Git+[jekyll](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)+[markdown](http://wowubuntu.com/markdown)；
Git管理内容的提交版本等；jekyll为Ruby项目，负责静态网站生成引擎；markdown是其支持的文章内容格式，环境搭建好后，我们只需要专注于Markdown编写Blog内容文章。

在开始之前，必需先具有GitHub帐号，Git客户端。

## 三 第一个页面

开始构建Blog

### 3.1 Git配置

首先是配置ssh key，以便连接，在Git Bash(下绪的命令操作都可在Git Bash下)中

>`$ ssh-keygen -t rsa -C "your_email@address.com"` 

_"your_email@address.com"_为帐号邮箱，此后会提示输入key保存文件，自定义即可(假设为*"id_rsa"*)；以及输入密码passphrase(可有可无)；成功后会在用户目录下生成_/.ssh_
文件夹(windows系统为C:\users\username\.ssh)，内有刚刚生成的密钥文件*"id_rsa","id_rsa.pub"*。打开*"id_rsa.pub"*文件复制全部内容，在GitHub网站中进入Profile设置界面 Edit Profile，
在setting中选择*SSH and GPG keys*,*New SSH key*增加Key,为我们的客户端添加信任,自定义title，粘贴key，确认即可。

此时可通过以下验证

> `$ ssh -T git@github.com`

首次建立连接会提示是否继续，同时将ip加入本地host信任，yes后会得到successfullly成功提示。

为了避免每次提交时都输入帐号密码的麻烦，此时可设置github username和email

>   `$ git config --global user.name "your username"`           
    `$ git config --global user.mail "your_email@address.com"`
   
### 3.2 Git repository

在GitHub网站中新建好*username.github.io*仓库后，在本地切换到你想保存项目的位置下，clone repository并进入仓库目录

>`$ git clone https://github.com/username/username.github.io`      
 `$ cd username.github.io`
 
*username.github.io*仓库也可以fork你感兴趣的GitHub pages博客模板项目，GitHub上有各种方格可供选择，在此基础上修改更为方便。
Clone到本地后，即可在本地编辑博客Git commit push提交后可看到网站结果

简单的测试：

> `$ echo "Hello World" > index.html`    
  `$ git add --all`    
  `$ git commit -m "commit comment"`    
  `$ git push -u origin master`
 
浏览器访问 *http://username.github.io* 可看到”Hello World"页面

如果不使用Clone到本地，我们也可以*git add*添加远程地址并commit push上传本地文件

> `$ git init // not necessary`    
> `$ git remote add origin git@github.com:username/username.github.io.git`

### 3.3 页面生成

如上例，我们可以直接按Html语法写*html*文件，发布到GitHub远程，这样太过麻烦，当然也可以使用其它工具写好页面再提交发布。
GitHub也提供了*automatic page generator*支持markdown编写并内置若干布局,在repo的settings找到*launch automatic page generator*。
![page generator](/img/post-2016/github_pages/page_generator.jpg)
![new page](/img/post-2016/github_pages/new_page.jpg)

我们要使用的是jekyll来提供更加丰富的功能和便利的博客编辑

## 四 jekyll 

jekyll引擎使用一个模板目录作为网站布局的基础框架，支持Markdown、Textile等标记语言的解析，提供变量、模板、组件等功能，可以生成完整的站点。

### 4.1 安装jekyll本地环境

本地环境可以用本地效果预览，首先下载[RubyInstaller](http://rubyinstaller.org/downloads)，安装并确保添加Path环境变量

> `$ ruby -v // make sure ruby installed`     
  `$ gem update --system // update gem`

windows系统应安装[DevKit](http://rubyinstaller.org/downloads)以确保后绪某些库的成功安装,DevKit是windows平台下编译和使用本地C/C++扩展包的工具,
用来模拟Linux平台下的make,gcc,sh来进行编译。下载Devkit,安装exe

> `$ cd c:\Devkit // the dev kit install directory`    
  `$ ruby dk.rb init`    
  `$ ruby dk.rb install`  
  
如果 init 后，不能自己找到ruby目录，我们还需要在生成的*config.yml*文件内手动加入ruby安装目录(注意*---*和*-*)

> \---    
  \- c:/ruby  # direcory of ruby

Install如果没有error提示并已安装完成，此后可以安装jekyll及其它辅助库

> `$ gem install bundler # Installs the Bundler gem`
 
在当前repo文件夹中创建*Gemfile*(如果使用了他方主题存在gemfile则不需要)，在其中加入

>  source 'https://rubygems.org'    
gem 'github-pages', group: :jekyll_plugins

安装jekyll和依赖库

> `$ bundle install`    
Fetching gem metadata from https://rubygems.org/............    
Fetching version metadata from https://rubygems.org/... 
Fetching dependency metadata from https://rubygems.org/..   
Resolving dependencies...

如无意外，至此本地环境已经安装成功

### 4.2 运行jekyll生成网站

repo中存在jekyll项目时，可运行jekyll在本地浏览器中查看效果

> `$ bundle exec jekyll serve` // only 'jekyll serve' ok too

如果没有已有项目存在，创建新的jekyll模板站点

> `$ bundle exec jekyll new . --force`

默认站点地下是* http://127.0.0.1:4000/ *如果提示端口*Access denied*，说明端口已被在，可在项目
的*\_config.yml*配置文件中更改加入`port: portnumber`，重新运行生成。

任何本地的更改，*git push*到github后可生效博客更新

## gitignore

.gitignore是告诉git需要忽略的文件，这是一个很实用的设置文件。写完代码后会执行编译、调试等操作，这期间会产生很多中间文件和可执行文件，这些都是不需要git来管理的。
需要添加到.gitignore文件中。

在kekyll项目中 

>_site  
node_modules  
npm-debug.log   
.DS_Store            
\*/.DS_Store         
\*/\*/.DS_Store       
\*.sh  


## Reference
- [Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)
- [如何搭建一个独立博客——简明Github Pages与Hexo教程](http://www.jianshu.com/p/05289a4bc82b2)
- [Github 简明教程](http://www.runoob.com/w3cnote/git-guide.html)
- [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门 by 阮一峰](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
- [一步步在GitHub上创建博客主页 全系列 by pchou](http://pchou.info/web-build/2013/01/03/build-github-blog-page-01.html)
 
 


 








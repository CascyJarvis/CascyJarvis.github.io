---
layout: post
title: "Starting with Octopress"
date: 2014-07-09 12:24:38 +0800
comments: true
categories: 
---

[Octopress](http://octopress.org/)是利用[Jekyll](https://github.com/jekyll/jekyll)博客引擎开发的一个博客系统，可以将生成的静态页面在github page上进行很好的展现。号称是hacker专属的一个博客系统(A blogging framework for hackers.)
搭建Octopress需要读者熟悉一些shell命令，并掌握git的基本操作。
[官方文档](http://octopress.org/docs/)

<!--more-->

--------
####安装Ruby
Mac系统自带有Ruby环境，无需特别的安装即可使用，如果没有Ruby环境，可以参考[Installing Ruby With RVM](http://octopress.org/docs/setup/rvm/)进行安装  

* 安装RVM   

```
curl -L https://get.rvm.io | bash -s stable --ruby
```

* 安装Ruby 1.9.3
    
```
rvm install 1.9.3
rvm use 1.9.3
rvm rubygems latest
```

完成上面的操作之后，运行`ruby --version`应该可以看到ruby 1.9.3环境已经安装好了。

--------
####安装Octopress
* 将Octopress源码从GitHub上clone下来

```
git clone git://github.com/imathis/octopress.git octopress    # 从github clone octopress的源代码，后面的octopress是本地存放文件夹的名称，可以自定
cd octopress
```

* 安装依赖

```
gem install bundler
rbenv rehash    # If you use rbenv, rehash to be able to run the bundle command
bundle install
```


-------
####博客部署

* 创建一个user_name.github.com（或者user_name.github.io）的repo（必须使用此种命名方式），GitHub会需要几分钟的时间为你生成显示的页面，在部署完成后，我们就可以在user_name.github.com（或user_name.github.io）的页面上看到我们的博客信息了。

```
rake setup_github_pages # 和github创建关联
git@github.com:your_username/your_username.github.com.git   # 按提示输入github URL
rake generate # 把你所有编辑的内容生成你的Blog静态页面
rake deploy   # <span class="goog_qs-tidbit goog_qs-tidbit-0">如果检查没有任何问题就可以 push 你的 blog 到 github master branch</span> 
＃ 状态检查
cd ~/octopress
git status   # 应该显示 On branch source
cd _deploy/  # 应该显示 On branch master
＃ 最后提交到source branch
git add .
git commit -m 'first commit'
git push origin source # 如果这一步出错，请再次检查仓库名称是否按要求命名，同时检查Admin面板下Default Branch是否
```
> 最终生成的包括Octopress的所有源码以及资源代码放到了source分支下，而_deploy目录下生成的静态页面则放到了master分支下


-------
####撰写博客
撰写博客主要会使用到以下几个命令：

* `rake new_post[‘article name’]`生成博客模板文件，文件位置在source/_posts/目录下，修改相应文件即可
* `rake generate`生成静态页面，页面放在_deploy目录下
* `rake watch` 检测文件变化，实时生成新内容
* `rake preview` 在本机生成预览页面，ctrl+c可退出预览状态，预览页面可在<localhost:4000>浏览
* `rake deploy` 将生成的静态页面部署到git服务器

博客的页面是使用markdown语法进行书写的，网上关于markdown的教程很多，推荐使用[Mou](http://mouapp.com/)进行博客的编写。
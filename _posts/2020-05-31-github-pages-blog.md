---
layout: post # 页面类型
title: GitHub Pages 博客 # 标题
subtitle: 教大家如何创建一个和我这个一样的博客 # 子标题
cover-img: /img/githubpages/cover.png # 封面图片
gh-repo: BobbyBBY/BobbyBBY.github.io
gh-badge: [star, fork, follow]
tags: [github,技术] # 标签
comments: true
catalog: true
---
这篇文章是教大家如何创建一个和我这个一样的博客。
GitHub Page，一般多用于托管个人的静态网站，所以现在很多人也用来它来搭建私人博客，也算是省去了购买服务器、域名等等一系列复杂的操作。搭建博客网站有各种各样的方法，像懂php的可以用WordPress，懂Java的可以用Jpress等等。如果你想简单和简约，那么我强烈推荐你使用Github Page。  

我写博客的目的大部分是为了做笔记，当然笔记一般都在onenote里，写到博客里就是一种更正式的整理，也可以方便大家。选择GitHub Pages的原因大抵就是不想被监管，并且能够随意修改风格。  

---

## 新建GitHub Pages

这一步从简介绍。  
  
首先准备好GitHub账户，没有的话请自行注册。然后新建一个仓库，这里仓库的名字一定要填写【你GitHub的昵称】.github.io。我的GitHub昵称是BobbyBBY，所以在下图蓝色方框位置填写 “ BobbyBBY.github.io  ”  

对于每个人来说这个仓库名是唯一的，这也是你博客的url（即网址）。可以看到我已经创建了GitHub Pages，这里就显示“已存在”。  

根据你的喜好添加描述（Description），添加介绍页（Initialize this repository with a README）  

最后生成仓库（create repository）。  

![repository](/img/githubpages/repository.png)  

新建时一定要选择公开仓库（Public），否则无法访问。如果不小心选择了私密仓库（Private），可以在以后的设置中调回来。但是改变公开性后，这个项目不会自动发布，需要在设置中点击“Change theme”（如下图），随便选择一个主题来重新发布项目，如果你的仓库中已经有内容，选择主题不会覆盖原有的内容（这是我尝试的结果，官方没有给出解答）  

![re-public](/img/githubpages/re-public.png)  

在仓库中新建一个index.html文件，在里面输入任意内容，然后再把代码推送到GitHub上，过大约几十秒再访问你的网页，可以看到网页展示了index.html里面的内容。  
  
请注意，只有master分支的项目会被部署。  

---

## 选择模板主题

GitHub Pages 可以使用Jekyll。jekyll是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，不需要数据库支持。但是可以配合第三方服务，例如Disqus。最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。（ [摘自](https://baike.baidu.com/item/jekyll/1164861?fr=aladdin)）  

默认创建的博客模板一般比较简单，jekyll官网提供了大量博客模板，我们可以去挑选一个自己喜欢的博客模板，或者在github全网搜索“jekyll”，然后在其基础上进行修改。如果你对html、css、js不是很熟悉，只需要认真挑选一个模板，然后专注写博文就可以了。  

* 官方jekyll主题 [http://jekyllthemes.org/](http://jekyllthemes.org/)  
* 第三方jekyll主题 [http://themes.jekyllrc.org/](http://themes.jekyllrc.org/)  
* 我使用的主题是 [https://github.com/daattali/beautiful-jekyll](https://github.com/daattali/beautiful-jekyll)  

将主题下载下来，并将内容直接拷贝至你的仓库目录中（注意别复制.git文件夹），然后推送代码至GitHub即可。

博文使用[markdown语法](https://www.runoob.com/markdown/md-title.html)排版，学习起来非常方便。由于几乎所有jekyll模板都使用markdown语法排版博文，博文是互相兼容的，后期可以任意切换模板。  

---

## 本地部署调试

调试网页有两种方法：  

1. 修改代码-推送GitHub-等待-查看网页。  
2. 本地安装编译工具后，修改代码-查看网页，待满意后推送GitHub  

第一种方法适合小白，不过太耗时。第二种方法调试网页很方便，但是需要安装编译工具，安装时很容易遇到问题。  

jekyll相当于一个编译工具，安装好jekyll后，你可以通过jekyll创建一个网站服务器，创建好之后，就可以通过[http://127.0.0.1:4000/](http://127.0.0.1:4000/)访问刚刚创建的网站了。你可以修改博文或者网站设置，并通过本地url实时预览改动后的效果。  
  
**windows下的安装步骤：**  

1. 首先点击下载安装Ruby installer，路径不要出现空格及中文，最好安装在根目录 [https://rubyinstaller.org/](https://rubyinstaller.org/)
2. 点击下载RubyGems [https://rubygems.org/pages/download](https://rubygems.org/pages/download)
3. 下载完成后解压至你想放的位置，在该位置打开命令行或powershell执行：  

```shell
ruby setup.rb
gem install jekyll
```

1、安装完成后，在一个新的目录下会生成一个重要的中间文件。即使你使用的别人的模板，也请不要省略这一步，后面很有可能会用得到。  

```shell
jekyll new testblog
cd testblog
jekyll server
```

在浏览器输入[http://127.0.0.1:4000/](http://127.0.0.1:4000/)即可浏览刚刚创建的testblog（[摘自](https://www.jianshu.com/p/9f71e260925d)）  

2、关闭终端即可取消部署  

切换到你的博客的本地仓库地址，命令行执行以下命令即可部署你的博客

```shell
jekyll server
```

如果使用了第三方模板，部署过程可能会遇到一些问题  

如出现了以下错误提示  

![error](/img/githubpages/error.png)  

解决方案一（推荐）  

将之前创建的testblog中的Gemfile，Gemfile.lock复制到你的博客中  

解决方案二  

```shell
gem install [软件名]
```

这个软件名即 “ coul not find gem '[软件名]' ”  

有时候需要特定的版本  

```shell
gem install [软件名] -v 2.3.4
```

至此你就可以慢慢折腾你的博客了(*^_^*)  

---

## 相关网页

GitHub Pages官方教程 [https://pages.github.com/](https://pages.github.com/)  
markdown语法 [https://www.runoob.com/markdown/md-title.html](https://www.runoob.com/markdown/md-title.html)  
jekyll中文网 [http://jekyllcn.com](http://jekyllcn.com)  
部分参考教程 [https://www.jianshu.com/p/9f71e260925d](https://www.jianshu.com/p/9f71e260925d)  

---
layout: post
title: 使用Jekyll搭建静态博客[转]
---

{{ page.title }}
================

<p class="meta">6/1/2016 2:43:17 PM  - 北京</p>



----------
Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 Markdown）和我们的 Liquid 渲染器转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Page 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。


<!--more-->

jekyll是基于ruby的,需要ruby环境。

**一、安装Jekyll**

- 1、安装Homebrew
	
	 Homebrew是什么？
	linux系统有个让人费神的通病，软件包依赖，好在当前主流的两大发行版本都自带了解决方案，Red hat有yum，Ubuntu有apt-get
	Mac os有Homebrew。
	Homebrew简称brew，是Mac OSX上的软件包管理工具，能在Mac中方便的安装软件或者卸载软件，可以说Homebrew就是mac下的apt-get、yum神器
	
	Homebrew使用
	
	
	常用的命令
	
	
	搜索软件：brew search 软件名，如brew search wget
	
	
	安装软件：brew install 软件名，如brew install wget
	
	
	卸载软件：brew remove 软件名，如brew remove wget
	
	
	Homebrew的安装非常简单，打开终端复制、粘贴以下命令，回车，OK(官方安装方法搬运）

	`ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/
	master/install)"`


- 2、Mac 下使用 Homebrew 来安装ruby，很方便。
	
	`$ brew install ruby`

	安装完之后，在终端检查是否安装好。运行ruby -v，看看输入的日志是否为ruby 1.9.3p327，后面的版本号跟你安装的版本有关。
	
	目前 Jekyll 最新版本需要 Ruby 1.9.1 以上。

- 3、安装RubyGems

	RubyGems 是一个 Ruby 包的管理工具，就像 Homebrew，npm 等，可以下载包到本地。
	RubyGems（简称 gems）是一个用于对 Ruby组件进行打包的 Ruby 打包系统。 它提供一个分发 Ruby 程序和库的标准格式，还提供一个管理程序包安装的工具。
	RubyGems的功能类似于Linux下的apt-get。
	
	
	可能需要根权限-如果权限不够就使用 sudo gem update —system



	`$ gem update --system`



	检查当前版本


	`$ gem -v`


	如果源不好用的话


	更换源
    
    `$ gem sources --remove https://rubygems.org/`
    
    `$ gem sources -a http://ruby.taobao.org/`

- 4、安装jekyll

	`gem install jekyll`
	
	在这一步出了问题，报错ERROR: Failed to build gem native extension.
	
	解决方法是安装安装Xcode Command Line Tools
	
	`$ sudo xcode-select --install`
	`$ xcode-select --print-path`

	然后就可以正常安装了

**二、使用Jekyll并部署到github上**

演示如何在github上搭建blog。这个blog只有最基本的功能。
在搭建之前，你必须已经安装了Git，并且有github账户。

- 1、新建网站：执行jekyll new myjekyll会在当前的目录下创建一个myjekyll的目录，里面就是网站的所有文件
对该目录进行git初始化。
	
	`$ cd myjekyll`
	
	`$ git init`

创建一个没有父节点的分支gh-pages。因为github规定，只有该分支中的页面，才会生成网页文件。

	git checkout --orphan gh-pages

发布内容。
先把所有内容加入本地git库。

	$ git add
	$ git commit -m "first post"

然后，前往github的网站，在网站上创建一个名为myjekyll的库。接着，再将本地内容推送到github上你刚创建的库。注意，下面命令中的username，要替换成你的username。
	
	$ git remote add origin https://github.com/username/myjekyll.git
	$ git push origin gh-pages

上传成功之后，等10分钟左右，访问http://username.github.com/myjekyll/就可以看到Blog已经生成了（将username换成你的用户名）

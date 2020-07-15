---
layout: post_layout
title:  「NodeJS」升级Node js和npm
time: 2018年05月23日 星期三 
location: 北京
pulished: true
excerpt_separator: "```"
---

一行命令搞定npm和node.js的升级，省去了重新编译和安装的过程。具体如下：

#### 升级node.js
npm中有一个模块叫做“n”，专门用来管理node.js版本的。

<!--more-->

更新到最新的稳定版只需要在命令行中打下如下代码：
    
```bash
npm install -g n
n stable
```


如需最新版本则用n latest

屏幕快照 2017-04-11 上午10.02.08


当然，n后面也可以跟具体的版本号：n v6.2.0

node.js升级就是这么简单。

#### 升级npm

npm升级就更简单了，只需要在终端中输入：

```bash
npm -g install npm@next
```

也可以用这个命令：

```bash
npm install npm -g
```
---
layout: post_layout
title: 「MyBatis」聊聊MyBatis缓存机制
time: 2018年05月22日 星期二
location: 北京
pulished: true
excerpt_separator: "```"
---

### 前言


MyBatis是常见的Java数据库访问层框架。在日常工作中，开发人员多数情况下是使用MyBatis的默认缓存配置，但是MyBatis缓存机制有一些不足之处，在使用中容易引起脏数据，形成一些潜在的隐患。个人在业务开发中也处理过一些由于MyBatis缓存引发的开发问题，带着个人的兴趣，希望从应用及源码的角度为读者梳理MyBatis缓存机制。

    
本次分析中涉及到的代码和数据库表均放在GitHub上，地址： [mybatis-cache-demo](https://github.com/kailuncen/mybatis-cache-demo) 。

    

> 本文转自[美团技术博客](https://tech.meituan.com/mybatis_cache.html)


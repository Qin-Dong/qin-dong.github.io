---
layout: post
title:  "Gitalk评论插件设置详解"
categories: GitHub
tags: GitHub
author: QinDong
---

* content
{:toc}

Gitalk 是一个基于 Github Issue 和 Preact 开发的评论插件。为什么我要写这篇博文，是因我用Github搭建我的博客页面，在安装Gitalk插件时按网上文章介绍进行配置，在运行时总是出现错误：
![](/img/2019/201909070101.png)

每篇文章都只突出解释了某一条设置，如有文章在说到“repo”变量时提到是库名，有一篇文章更是说是博客地址，这几种说法貌似也对，但严格来说却并不正确，而正确的设置是类似于“qin-dong.github.io”的库名。不管是带上.git的库地址还是带上https的博客地址都是不正确的。正确的设置如下：

Gitalk评论效果参见本人github页面：
![](/img/2019/201909070102.png)
![](/img/2019/201909070103.png)
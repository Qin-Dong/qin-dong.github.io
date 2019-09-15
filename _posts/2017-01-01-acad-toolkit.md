---
layout: post
title:  "AutoCAD工程测量工具集"
categories: 测绘 AutoCAD
tags: AutoCAD AutoLISP 工程测量 面积
author: QinDong
---
* content
{:toc}

### 加载

AutoLisp程序可用appload命令加载。

![](/img/2017/20170101-acad-toolkit-01.png)

推荐加载时加入自启动组。

![](/img/2017/20170101-acad-toolkit-02.png)

![](/img/2017/20170101-acad-toolkit-03.png)

所有命令均以“zz”开头，程序加载后在命令行中输入“zz”就会弹出所有命令列表，用上、下键在列表中选择后按回车键执行即可。

![](/img/2017/20170101-acad-toolkit-04.png)

### 面积标注

执行命令：zza、zzarea
将封闭区域的面积直接标在区域内。在命令行输入：zza或zzarea

![](/img/2017/20170101-acad-toolkit-05.png)

输入面积编号或名称“a1”，在需求面积的区域内点击即可：

![](/img/2017/20170101-acad-toolkit-06.png)

![](/img/2017/20170101-acad-toolkit-07.png)

图上求面积区域显示:“a1:103.61”

### 求面积至CAD表中

执行命令：zzArea2Table （注意：低版本不支持表对象）

![](/img/2017/20170101-acad-toolkit-08.png)

按提示输入起始序号“1”，并指定面积表放置位置：

![](/img/2017/20170101-acad-toolkit-09.png)

将创建一个空的表对象：

![](/img/2017/20170101-acad-toolkit-10.png)

依次点击需求面积区域：

![](/img/2017/20170101-acad-toolkit-11.png)

![](/img/2017/20170101-acad-toolkit-12.png)

面积值将依次加入表中，且求面积区域将显示表中对应序号。

### 求面积并保存入文件中
输入命令：ZZAREA2FILE，提示输入保存面积文件名，后续操作与命令“zzArea2table”类似，求面积区域显示序号，存放面积的文本文件中以序号、面积逐行保存。

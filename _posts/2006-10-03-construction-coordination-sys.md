---
layout: post
title:  "测图坐标系与工程坐标系转换公式："
categories: 测绘
tags: 坐标系 公式
author: QinDong
---
* content
{:toc}

### 1、测图坐标系->工程坐标系公式：		
xp=(Xp-Xo)cos(a)+(Yp-Yo)sin(a)+xo		
yp=-(Xp-Xo)sin(a)+(Yp-Yo)cos(a)+yo		
		
### 2、工程坐标系->测图坐标系：		
Xp=Xo+(xp-xo)cos(a)-(yp-yo)sin(a)		
Yp=Yo+(xp-xo)sin(a)+(yp-yo)cos(a)		

### 说明
Xp、Yp：待求点测图坐标
xp、yp：待求点工程坐标
Xo、Yo：工程坐标系原点测图坐标
xo、yo： 工程坐标系原点工程坐标
a：工程坐标系x轴在测图坐标系中的坐标方位角

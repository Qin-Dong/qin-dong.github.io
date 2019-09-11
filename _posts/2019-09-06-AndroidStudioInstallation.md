---
layout: post
title:  "Android Studio开发环境安装亲历记"
categories: 安卓
tags: Android Kotlin Java
author: QinDong
---

* content
{:toc}

## 安装过程
安卓开发环境主要以Eclipse+ADT和Android Studio（简称AS）两种为主，当然也可以在其它IDE上通过插件实现。而目前Google已停止对ADT的开发支持，全力推广自己的AS。进行过安卓开发的同仁们对刚接触时安卓开发环境的搭建记忆应该比较深刻，其过程可称得上紧张刺激且痛苦，搞掂之后还能给你来点快感。开发环境基本分三大块：JAVA环境（JDK）、IDE（Eclipse+ADT、Android Studio）、Android环境（SDK）。如果是现在刚准备着手学习安卓开发的童鞋，Eclipse作为已被谷歌淘汰的工具可以不予考虑，而应该全力学习用AS进行开发。

由于受网络（墙）的影响，AS的安装过程是非常痛苦的。它跟以往的软件安装过程不太一样，安装好之后在运行阶段还要连到网上下一大堆东西，而国内没有好的镜像支持，多数情况会无休止的尝试连接、出错，会把你的一点点耐心消磨得干干净净，最后把头脑中一时兴起的安卓开发大业一弃了之。

本人在安装AS时，头一天足足折腾了一整天，晚上整到深更半夜，最后还是带着满满的失望上床睡觉去了。睡觉前也曾想过装不起来就卸了它。但好在我在这么多年的编程过程中总结出了一条规律，不管什么艰难的问题，过一段时间一天或是几天后总会能把它搞定，现在的问题以后再来回头看就不再是问题。于是决定先把AS的僵尸姑且先放在电脑上再说，待明天或以后再抽时间倒腾一下再说。

第二天上班后，把电脑带到办公室，开启继续折腾模式。把AS下面出错时显示的错误提示信息几乎都拷出来在网上搜索过一遍。CSDN里倒是有一大堆类似的问题和解决办法，但按上面所说总是不能凑效。主要是SDK的下载安装与Gradle更新。安卓SDK倒是不陌生，但那个gradle一直没有摸透是个什么东东，到现在也不道是干什么使的，只是知道AS离了它啥也干不了。

一直耐心地在网上搜索各种问题的解决办法，快到中午下班前的一次Build时，提示窗口一路打勾，直到出现Run Task，才终于如释重负，AS终于被老夫搞定。搞定之后静下心来仔细想了下是什么地方起了关键作用呢？问题搞定了都不知是咋搞定的，但有一步我认为可能是关键，就是在设置Proxy时，前面使用的都是CSDN里或网上其它博客中提到的Proxy地址，直到在浏览GitHub时看到一个代理地址列表，死马当活马医地把最前面几个中的一个中国的IP地址和端口设置到AS中，可能是这步起了关键作用。

![](/img/2019/20190906092148737.png)

现在总结下安装步骤：

1、JAVA JDK的安装：[https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，下载的8u221 Windows 64版；

2、Android Studio下载：[https://developer.android.google.cn/studio](https://developer.android.google.cn/studio) 上下载最新版，其它地方的也下载过，但最后是在这下载的3.4版（到我写此文时仅过了一天，今天上去看就变成3.5版的了，呵呵）；

3、Android SDK：[https://www.androiddevtools.cn/index.html#sdk-tools](https://www.androiddevtools.cn/index.html#sdk-tools) （其实在网通的情况下AS是可以自己下载SDK的，只是没这个SDK tools下载得快）下载SDK Tools安装后运行，选择几个常用的SDK下载（我只选了8.0版），然后将下载的SDK拷贝或移动到某个驱动器下的“android-sdk”目录中，这是因为在AS的某一步中明确提示SDK库的路径中不能包含空格字符。上面这个地址的SDK Tools可能是为Eclipse准备的。将SDK下载一两个版本后，运行AS，按提示操作，更新后就可以了。

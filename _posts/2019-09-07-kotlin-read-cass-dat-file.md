---
layout: post
title:  "kotlin逐行读取CASS地形数据文件坐标值"
categories: kotlin
tags: kotlin CASS 地形
author: QinDong
---

* content
{:toc}

[TOC]

## 体会
kotlin有着JAVA般强大的功能，却象VB一样简单人性化。真是很好的一门语言。因考虑到安卓强大的市场与广阔的前景，掌握其应用开发将是大有裨益的。于是着手将电脑上的安卓开发环境又重装了起来，虽然因网络不畅和SDK庞大的体积，安装开发环境是一件严竣的挑战，但经过一天多时间终于还是将其安装好了。下面是盼望已久的“hello World!”

![](/img/2019/201909070201.jpg)

## 代码

``` kotlin
package hello
 
import java.io.File
import kotlin.math.cos
import kotlin.math.sign
 
//  可选的包头
 
fun main(args: Array<String>) {    // 包级可见的函数，接受一个字符串数组作为参数
    var tst:Int
    tst=add(10,26)
    println(tst)
    Greeter("World!").greet()
    readFile()
    //println("Hello World! $tst My name is QinDong")         // 分号可以省略
}
 
fun add(x: Int, y: Int): Int {
    return x+y
}
 
class Greeter(val name:String){
    fun greet(){
        println("Hello $name")
    }
}
 
fun readFile(){
    val filename="""d:\地形图坐标方格网数据.dat"""
    val file= File(filename)
    //val contents=file.readText()
    //println(contents)
    println(file.readLines()[0]) //读取第一行
    println(file.readLines()[0].split(",")[2]) //读取第一行E坐标
    val line=file.readLines()[3]
    println(line.split(",")[0]) //点号
    println(line.split(",")[1]) //代码
    println(line.split(",")[2]) //E
    println(line.split(",")[3]) //N
    println(line.split(",")[4]) //H
}
```
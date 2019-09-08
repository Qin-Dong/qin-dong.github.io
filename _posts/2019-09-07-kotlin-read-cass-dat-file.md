---
layout: post
title:  "kotlin逐行读取CASS地形数据文件坐标值"
categories: kotlin
tags: kotlin CASS 地形
author: QinDong
---

* content
{:toc}

## 体会
kotlin有着JAVA般强大的功能，却象VB一样简单人性化。真是很好的一门语言。

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
————————————————
版权声明：本文为CSDN博主「Qin Dong」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/hjpqindong/article/details/100589988
```
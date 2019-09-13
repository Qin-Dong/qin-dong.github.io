---
layout: post
title:  "Kotlin入门之字符串及其格式化"
categories: 安卓
tags: Kotlin 字符串
author: QinDong
---
* content
{:toc}

基本变量类型可以通过toString方法转为字符串类型。反过来，字符串类型又该如何转为基本变量类型？如果使用Java编码，有以下几种转换方式：
- 字符串转整型：Integer.parseInt(***)
- 字符串转长整型：Long.parseLong(***)
- 字符串转浮点数：Float.parseFloat(***)
- 字符串转双精度数：Double.parseDouble(***)
- 字符串转布尔型：Boolean.parseBoolean(***)
- 字符串转字符数组：调用String对象的toCharArray方法




就上面的转换情况来看，Java的实现方式比较繁琐，既需要其它类型的类名，有需要其它类型的转换方法。而在Kotlin这边，转换类型相对简单，并且与基本变量类型之间的转换保持一致，具体说明如下：
- 字符串转整型：调用String对象的toInt方法
- 字符串转长整型：调用String对象的toLong方法
- 字符串转浮点数：调用String对象的toFloat方法
- 字符串转双精度数：调用String对象的toDouble方法
- 字符串转布尔型：调用String对象的toBoolean方法
- 字符串转字符数组：调用String对象的toCharArray方法

显而易见，Kotlin对字符串的类型转换更友好，也更方便记忆。

当然，转换类型只是字符串的基本用法，还有更多处理字符串的其他用法，比如查找子串、替换子串、截取指定位置的子串、按特定字符分隔子串等等，在这方面Kotlin基本兼容Java的相关方法。对于查找子串的操作，二者都调用indexOf方法；对于截取指定位置子串的操作，二者都调用substring方法；对于替换子串的操作，二者都调用replace方法；对于按特定字符分隔子串的操作，二者都调用split方法。
下面是Kotlin使用indexOf和substring方法的代码例子：
``` kotlin
    val origin:String = tv_origin.text.toString()
    var origin_trim:String = origin
    if (origin_trim.indexOf('.') > 0) {
        origin_trim = origin_trim.substring(0, origin_trim.indexOf('.'))
    }
```
在这些字符串处理方法里面，唯一的区别是split方法的返回值，在Java中，split方法返回的是String数组，即String[]；但在Kotlin中，split方法返回的是String队列，即List<String>。下面是Kotlin使用split方法的示例代码：
``` kotlin
    btn_split.setOnClickListener {
        var strList:List<String> = origin.split(".")
        var strResult:String = ""
        for (item in strList) {
            strResult = strResult + item + ", "
        }
        tv_convert.text = strResult
    }
```
若想获取字符串某个位置的字符，这个看似简单的需求，但Java实现之时却有点繁琐，只能调用substring方法去截取指定位置的字符串，如下所示：
``` kotlin
    String result = origin.substring(number, number+1);
    tv_convert.setText(result);
```
现在使用Kotlin实现上述需求，就简单多了，因为Kotlin允许直接通过下标访问指定位置的字符，代码如下：
``` kotlin
    tv_convert.text = origin[number].toString()
```
同时，Kotlin也支持字符串通过get方法获取指定位置上的字符，代码如下：
``` kotlin
    tv_convert.text = origin.get(number).toString()
```
如此一来，Kotlin的代码不但更加精炼，而且可读性也增强了。

Kotlin对字符串带来的便利并不限于此，大家知道，Java如果要把几个变量拼接成字符串，要么用加号强行拼接，要么用String.format函数进行格式化。可是前者的拼接加号，时常会跟数值相加的加号混淆；而后者的格式化，还得开发者死记硬背诸如%d、%f、%s、%c、%b等等格式转换符，实在令人头痛。对于格式化这个痛点，Kotlin恰如其分地进行了优化，何必引入这些麻烦的格式转换符呢？直接在字符串中塞进“$变量名”表示此处引用该变量的值，岂不妙哉！
心动不如行动，赶紧动起手来，看看Kotlin如何格式化字符串，代码如下所示：
``` kotlin
    btn_format.setOnClickListener { tv_convert.text = "字符串值为 $origin" }
```
这里要注意，符号$后面跟变量名，系统会自动匹配最长的变量名。比如下面这行代码，打印出来的是变量origin_trim的值，而不是origin的值：
``` kotlin
    btn_format.setOnClickListener { tv_convert.text = "字符串值为 $origin_trim" }
```
另外，有可能变量会先进行运算，再把运算结果拼接到字符串中。此时，则需用大括号把运算表达式给括起来，具体代码如下：
``` kotlin
    btn_length.setOnClickListener { tv_convert.text = "字符串长度为 ${origin.length}" }
```
注意到在Kotlin中，美元符号$属于特殊字符，因此不能直接打印它，必须经过转义才可打印。转义的办法是使用“${'***'}”表达式，该表达式外层的“${''}”为转义声明，内层的“***”为需要原样输出的字符串，所以通过表达式“${'$'}”即可打印一个美元符号，示例代码如下所示：
``` kotlin
    btn_dollar.setOnClickListener { tv_convert.text = "美元金额为 ${'$'}$origin" }
```
如果只是对单个美元符号做转义，也可直接在符号$前面加个反斜杆，即“\$”，代码如下：
``` kotlin
    btn_dollar.setOnClickListener { tv_convert.text = "美元金额为 \$$origin" }
```
不过一个反斜杆仅仅对一个字符进行转义，如果要对一个字符串做转义，也就是把某个字符串的所有字符原样输出，那么只能采用形如“${'***'}”的表达式了，该表达式用单引号把待转义的字符串包起来，好处是能够保留该字符串中的所有特殊字符。

下面来个Kotlin处理字符串的效果动图：

![](/img/2019/201909110201.gif)
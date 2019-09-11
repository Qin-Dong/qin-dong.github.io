---
layout: post
title:  "Kotlin基础之文件读、写"
categories: 安卓
tags: Kotlin 文件
author: QinDong
---
* content
{:toc}

Kotlin的IO操作都在kotlin.io包下。Kotlin的原则就是Java有的就直接利用，没有的才自己想办法写。因此Kotlin的IO操作主要是依靠扩展方法，在原有的Java类上添加功能实现的。这一点倒是和Groovy有点像。

下面介绍的很多方法都有一个Charset参数，可以直接要使用的字符集，默认是UTF-8。如果需要其他的字符集，用这个参数指定就行了。

### 终端IO
如果学过C++的同学可能会对Java超长的输出语句System.out.println()所震惊。同样的工作在C++里面只需要简单的cout<<就可以完成。

幸好，在Kotlin里面很简单，只需要使用println或者print这两个全局函数即可。我们不再需要冗长的前缀。

从终端读取数据也很简单，最基本的方法就是全局函数readLine，它直接从终端读取一行作为字符串。如果需要更进一步的处理，可以使用Kotlin提供的各种字符串处理函数来处理和转换字符串。

### 文件IO
Kotlin为java.io.File提供了大量好用的扩展方法，详细的扩展方法见这里java.io.File。这里我就跳着说几个最常用最好用的吧。

首先先看读取文件。如果需要简单读取一个文件，可以使用readText()方法，它直接返回了整个文件内容。如果希望按行读取，还可以使用readLines()方法，这会返回一个行字符串数组，我们可以随意操作。如果希望直接操作字节数组，那就使用readBytes()。如果想使用传统的Java方式，Kotlin也能满足你。

``` kotlin
fun readFile() {
    val filename = """C:\Windows\System32\drivers\etc\hosts"""
    val file = File(filename)
    val contents = file.readText()
    println(contents)
 
    //大写前三行
    file.readLines().take(3).forEach {
        println(it.toUpperCase())
    }
 
    //直接处理行
    file.forEachLine(action = ::println)
 
    //读取为bytes
    val bytes: ByteArray = file.readBytes()
    println(bytes.joinToString(separator = ""))
 
    //直接处理Reader或InputStream
    val reader: Reader = file.reader()
    val inputStream: InputStream = file.inputStream()
    val bufferedReader: BufferedReader = file.bufferedReader()
}
```
和读取文件类似，写入文件也很简单。我们可以写入字符串，也可以写入字节流。还可以直接使用Java的Writer或者OutputStream。
``` kotlin
fun writeFile() {
    val currentDir = System.getProperty("user.dir") + "\\out"
    val file = File(currentDir, "hehe.txt")
    file.writeText("呵呵呵哈哈哈")
    println(file.readText())
 
    file.writeBytes(byteArrayOf(12, 56, 83, 57))
    println(file.readText())
 
    //追加方式写入字节或字符
    file.appendBytes(byteArrayOf(93, 85, 74, 93))
    file.appendText("吼啊")
    println(file.readText())
 
    //直接使用writer和outputstream
    val writer: Writer = file.writer()
    val outputStream: OutputStream = file.outputStream()
    val printWriter: PrintWriter = file.printWriter()
}
```

### 遍历文件树
和Groovy一样，Kotlin也提供了方便的功能来遍历文件树。遍历文件树需要调用扩展方法walk()。它会返回一个FileTreeWalk对象，它有一些方法用于设置遍历方向和深度，详情参见FileTreeWalk。

下面的例子遍历了指定文件夹下的所有可执行文件。

``` kotlin
fun traverseFileTree() {
    val systemDir = File("""C:\Windows""")
    val fileTree: FileTreeWalk = systemDir.walk()
    fileTree.maxDepth(1)
            .filter { it.isFile }
            .filter { it.extension == "exe" }
            .forEach(::println)
 
}
```

### 网络IO
Kotlin为java.NET.URL增加了两个扩展方法，readBytes和readText。我们可以方便的使用这两个方法配合正则表达式实现网络爬虫的功能。

下面第一个例子简单的获取了百度首页的源代码。第二个例子先获取了必应首页图片的XML格式信息，然后通过正则表达式和分组获取图片的相对URL并组合出实际URL，然后调用readBytes()方法读取到字节流并写入文件。
``` kotlin
fun readFromNet() {
    //返回百度首页
    val baidu = URL("http://www.baidu.com")
    val contents = baidu.readText()
    //println(contents)
 
    //获取必应首页图片并保存
    //获取XML格式图片信息
    val bing = URL("http://www.bing.com/HPImageArchive.aspx?format=xml&idx=0&n=1&mkt=en-US")
    val texts = bing.readText()
    //获取url地址和文件名
    val regex = Regex("""<url>(.*)</url>""")
    val result = regex.find(texts)
    val imageUrl = "http://www.bing.com" + result!!.groupValues[1]
    val filename = imageUrl.substring(imageUrl.lastIndexOf('/'))
    //写入文件
    val output = File(System.getProperty("user.dir") + "\\out", filename)
    val requestUrl = URL(imageUrl)
    output.writeBytes(requestUrl.readBytes())
 
}
```
在项目相应文件夹下我们可以看到下载好的图片。
![](/img/2019/201909110401-kotlin-file-read-write)
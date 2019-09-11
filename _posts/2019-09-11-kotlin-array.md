---
layout: post
title:  "Kotlin入门之声明与操作数组"
categories: 安卓
tags: Kotlin 数组
author: QinDong
---
* content
{:toc}

Kotlink中数组声明和定义：
``` kotlin
var string_array:Array<String> = arrayOf("How", "Are", "You")
var int_array:Array<Int> = arrayOf(1, 2, 3)
var long_array:Array<Long> = arrayOf(1, 2, 3)
var float_array:Array<Float> = arrayOf(1.0f, 2.0f, 3.0f)
var double_array:Array<Double> = arrayOf(1.0, 2.0, 3.0)
var boolean_array:Array<Boolean> = arrayOf(true, false, true)
var char_array:Array<Char> = arrayOf('a', 'b', 'c')
```

数组操作：
``` kotlin
btn_string.setOnClickListener {
var str:String = ""
    var i:Int = 0
    while (i<string_array.size) {
        str = str + string_array[i] + ", "
        //数组元素可以通过下标访问，也可通过get方法访问
        //str = str + string_array.get(i) + ", "
        i++
    }
    tv_item_list.text = str
}
```
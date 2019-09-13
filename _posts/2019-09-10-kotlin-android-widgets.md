---
layout: post
title:  "Kotlin Android开发常用控件用法"
categories: 安卓
tags: Kotlin Android 控件 TextView Button
author: QinDong
---
* content
{:toc}

## Button
``` xml
<Button  
    android:id="@+id/button"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:layout_below="@+id/textView4"  
    android:layout_centerHorizontal="true"  
    android:layout_marginTop="112dp"  
    android:text="Click Button" />
<EditText  
    android:id="@+id/editText"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:layout_alignBaseline="@+id/textView2"  
    android:layout_alignBottom="@+id/textView2"  
    android:layout_alignParentEnd="true"  
    android:layout_alignParentRight="true"  
    android:layout_marginEnd="21dp"  
    android:layout_marginRight="21dp"  
    android:ems="10"  
    android:inputType="textPersonName"  
    android:text="" />  
<TextView  
    android:id="@+id/textView"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:layout_alignLeft="@+id/editText"  
    android:layout_alignStart="@+id/editText"  
    android:layout_alignTop="@+id/textView3"  
    android:textSize="16sp"  
    android:text="TextView" />    
```




为按钮添加事件，当EditText控件的内容不为空时将TextView的内容设为EditText的内容。
``` kotlin
    button.setOnClickListener(){  
        val inputValue: String = editText.text.toString()  
        if (inputValue == null || inputValue.trim()==""){  
            Toast.makeText(this,"please input data, edit text cannot be blank",Toast.LENGTH_LONG).show()  
        }else{  
            textView.setText(inputValue).toString()  
        }  
    }  
```
### 为按钮控件添加点击事的方法:
1. 匿名内部类:
``` kotlin
override fun onCreate(savedInstanceState: Bundle?) {
   super.onCreate(savedInstanceState)
   setContentView(R.layout.activity_main)
   bt_click.setOnClickListener {
     Toast.makeText(this,"点击了",Toast.LENGTH_SHORT).show();
   }
 }
```

2. Activity实现全局OnClickListener接口:

``` kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {
 override fun onClick(v: View?) {
   when (v?.id) {
     R.id.bt_click ->
       Toast.makeText(this, "点击了", Toast.LENGTH_SHORT).show()
   }
 }
 
 override fun onCreate(savedInstanceState: Bundle?) {
   super.onCreate(savedInstanceState)
   setContentView(R.layout.activity_main)
   bt_click.setOnClickListener(this)
 }
}
```

3. 指定xml的onClick属性:

``` xml
<Button
   android:id="@+id/bt_click"
   android:layout_width="match_parent"
   android:layout_height="50dp"
   android:onClick="click"
   android:text="点击" />
```

``` kotlin
fun click(v: View?) {
   when (v?.id) {
     R.id.bt_click ->
       Toast.makeText(this, "点击了", Toast.LENGTH_SHORT).show()
   }
 }
 
 override fun onCreate(savedInstanceState: Bundle?) {
   super.onCreate(savedInstanceState)
   setContentView(R.layout.activity_main)
 }
 ```



## TextView

``` xml
<TextView  
    android:id="@+id/textView1"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:layout_alignLeft="@+id/editText"  
    android:layout_alignStart="@+id/editText"  
    android:layout_alignTop="@+id/textView3"  
    android:textSize="16sp"  
    android:text="TextView" />  
     <TextView  
    android:id="@+id/textView2"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:layout_alignEnd="@+id/editText"  
    android:layout_alignRight="@+id/editText"  
    android:layout_centerVertical="true"  
    android:text="reset output text" />     
```

``` kotlin
textView2.setOnClickListener(){  
    if (textView1.text.toString() == null || textView1.text.toString().trim()==""){  
        Toast.makeText(this,"clicked on reset textView,\n output textView already reset",Toast.LENGTH_LONG).show()  
    }else{  
        textView1.setText("").toString()  
    }  
    }  
```

## EditText

为EditText控件添加内容变更触发事件。控件定义代码如下：

``` xml
<EditText  
    android:id="@+id/editText"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:layout_alignBaseline="@+id/textView2"  
    android:layout_alignBottom="@+id/textView2"  
    android:layout_alignParentEnd="true"  
    android:layout_alignParentRight="true"  
    android:layout_marginEnd="21dp"  
    android:layout_marginRight="21dp"  
    android:ems="10"  
    android:inputType="textPersonName"  
    android:text="" />  
```

实现代码如下：

``` kotlin
editText.addTextChangedListener(object: TextWatcher{  
    override fun beforeTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {  
      //  TODO("not implemented") //To change body of created functions use File | Settings | File Templates.  
        Toast.makeText(applicationContext,"executed before making any change over EditText",Toast.LENGTH_SHORT).show()  
    }  //事件：变更前触发。
  
    override fun onTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {  
      //  TODO("not implemented") //To change body of created functions use File | Settings | File Templates.  
        Toast.makeText(applicationContext,"executed while making any change over EditText",Toast.LENGTH_SHORT).show()  
    }  //事件：变更时触发。
    override fun afterTextChanged(p0: Editable?) {  
      //  TODO("not implemented") //To change body of created functions use File | Settings | File Templates.  
        Toast.makeText(applicationContext,"executed after change made over EditText",Toast.LENGTH_SHORT).show()  
    }  //事件：变更后触发。
    })  
```
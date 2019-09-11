---
layout: post
title:  "Kotlin Android TextView and EditText"
categories: 安卓
tags: Kotlin Android TextView EditText
author: QinDong
---
* content
{:toc}

## TextView
Android TextView is a user interface which displays the text to the user.
A simple XML code of TextView in a layout is shown below.
``` xml
<TextView  
        android:id="@+id/text_view_id"  
        android:layout_height="wrap_content"  
        android:layout_width="wrap_content"  
        android:text="hello" />
```
  
We can get and modify the content of text view defined in the XML layout in Kotlin class file as:

``` kotlin
val textView = findViewById<TextView>(R.id.text_view_id)  
textView.setText("string").toString()  
val textViewValue = textView.text
```

## EditText
  
The EditText is a user interface which is used for entering and changing the text. While using edit text in XML layout, we must specify its inputType attribute which configures the keyboard according to input type mention.

The simple XML code of EditText in a layout is shown below.

``` xml
<EditText  
    android:id="@+id/editText_id"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:inputType="textPersonName"  
    android:text="" />  
```

We can get and modify the content of edit text defined in the XML layout in Kotlin class file as:

``` kotlin
val editText = findViewById<EditText>(R.id.editText_id)   
val editTextValue = editText.text  
```

## Example

In this example, we input the text value in ExitText and display its value in the TextView on clicking the Button.

We are also watching the changes made over EditText using addTextChangedListener () method and TextWatcher interface.

### activity_main.xml

In the activity_main.xml file add the following code.

``` xml
<?xml version="1.0" encoding="utf-8"?>  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    tools:context="example.javatpoint.com.kotlintextviewedittext.MainActivity">  
  
  
    <TextView  
        android:id="@+id/textView1"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignParentTop="true"  
        android:layout_centerHorizontal="true"  
        android:layout_marginTop="12dp"  
        android:text="TextView and EditText"  
        android:gravity="center"  
        android:textAppearance="@style/Base.TextAppearance.AppCompat.Large"/>  
  
    <TextView  
        android:id="@+id/textView2"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_below="@+id/textView1"  
        android:layout_marginTop="90dp"  
        android:layout_marginLeft="20dp"  
        android:layout_marginStart="20dp"  
        android:textSize="16sp"  
        android:text="Provide Input" />  
  
    <TextView  
        android:id="@+id/textView3"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_below="@+id/textView2"  
        android:layout_marginTop="50dp"  
        android:layout_marginLeft="20dp"  
        android:layout_marginStart="20dp"  
        android:textSize="16sp"  
        android:text="Display Output" />  
  
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
        android:id="@+id/textView4"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignLeft="@+id/editText"  
        android:layout_alignStart="@+id/editText"  
        android:layout_alignTop="@+id/textView3"  
        android:textSize="16sp"  
        android:text="TextView" />  
  
    <Button  
        android:id="@+id/button"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_below="@+id/textView4"  
        android:layout_centerHorizontal="true"  
        android:layout_marginTop="112dp"  
        android:text="Click Button" />  
  
    <TextView  
        android:id="@+id/textView5"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignEnd="@+id/editText"  
        android:layout_alignRight="@+id/editText"  
        android:layout_centerVertical="true"  
        android:text="reset output text" />  
</RelativeLayout>  
```

### MainActivity.kt

Add the following code in MainActivity.kt class. In this class, we are getting the value of the edit text and display it in text view by clicking the button. At the same time, we are also watching the changes made over EditText using addTextChangedListener () method and TextWatcher interface. To learn more about TextWatcher refers to https://www.javatpoint.com/android-edittext-with-textwatcher

``` kotlin
package example.javatpoint.com.kotlintextviewedittext  
  
import android.support.v7.app.AppCompatActivity  
import android.os.Bundle  
import android.text.Editable  
import android.text.TextWatcher  
import android.view.View  
import android.widget.TextView  
import android.widget.Toast  
import kotlinx.android.synthetic.main.activity_main.*  
  
class MainActivity : AppCompatActivity() {  
  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)  
  
        button.setOnClickListener(){  
            val inputValue: String = editText.text.toString()  
            if (inputValue == null || inputValue.trim()==""){  
                Toast.makeText(this,"please input data, edit text cannot be blank",Toast.LENGTH_LONG).show()  
            }else{  
                textView4.setText(inputValue).toString()  
            }  
        }  
        textView5.setOnClickListener(){  
            if (textView4.text.toString() == null || textView4.text.toString().trim()==""){  
                Toast.makeText(this,"clicked on reset textView,\n output textView already reset",Toast.LENGTH_LONG).show()  
            }else{  
                textView4.setText("").toString()  
            }  
        }  
        editText.addTextChangedListener(object: TextWatcher{  
            override fun beforeTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {  
              //  TODO("not implemented") //To change body of created functions use File | Settings | File Templates.  
                Toast.makeText(applicationContext,"executed before making any change over EditText",Toast.LENGTH_SHORT).show()  
            }  
  
            override fun onTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {  
              //  TODO("not implemented") //To change body of created functions use File | Settings | File Templates.  
                Toast.makeText(applicationContext,"executed while making any change over EditText",Toast.LENGTH_SHORT).show()  
            }  
            override fun afterTextChanged(p0: Editable?) {  
              //  TODO("not implemented") //To change body of created functions use File | Settings | File Templates.  
                Toast.makeText(applicationContext,"executed after change made over EditText",Toast.LENGTH_SHORT).show()  
            }  
        })  
    }  
}
```
  
## Output

![](/img/2019/20190911-kotlin-android-textview-and-edittext-output.png)

## Original URL
[https://www.javatpoint.com/kotlin-android-textview-and-edittext](https://www.javatpoint.com/kotlin-android-textview-and-edittext)
---
layout: post
title:  "Kotlin Android Button Example"
categories: Kotlin Android
tags: kotlin android button
author: QinDong
---
* content
{:toc}

## Kotlin Android Button
Using Kotlin, we can perform events on Android Button though different ways, using:
### 1. Implement the setOnClickListener of Button
``` kotlin
button1.setOnClickListener(){  
            Toast.makeText(this,"button 1 clicked", Toast.LENGTH_SHORT).show()  
        }  
```
### 2. Implement the View.OnClickListner and override its function
``` kotlin
button2.setOnClickListener(this)   
    . .  
    override fun onClick(view: View) {  
        //  TODO("not implemented") //To change body of created functions use File | Settings | File Templates.  
    }  
```
### 3. Adding the onClick attribute of Button in layout file and implement its function.
``` xml
<Button  
            android:onClick="clickButton"/>  
```

``` kotlin  
fun clickButton(v: View){  
    val mToast = Toast.makeText(applicationContext,"button 3 clicked", Toast.LENGTH_SHORT)  
    mToast.show()  
}  
```
### 4. Create a Button programmatically and set it on the layout
``` kotlin
button4.setLayoutParams(ViewGroup.LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT))  
        button4.setId(button4_Id)  
        button4.x = 250f  
        button4.y = 500f  
        button4.setOnClickListener(this)  
        constraintLayout.addView(button4)  
```
## Kotlin Android Button Example
![Output](/img/2019/20190911-kotlin-android-button-output.png)

In this example, we will create the Button and performs event on them. Clicking on the Button, display a toast message.
### activity_main.xml
Add the three Button from the Widgets palette in the activity_main.xml layout file. Its code is given below. The Button of id button3 added the onClick attribute and its function name is implemented in MainActivity class file.
``` xml
<?xml version="1.0" encoding="utf-8"?>  
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:id="@+id/constraintLayout"  
    tools:context="example.javatpoint.com.kotlinbutton.MainActivity">  
    <TextView  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Button Action Example"  
        app:layout_constraintBottom_toBottomOf="parent"  
        app:layout_constraintLeft_toLeftOf="parent"  
        app:layout_constraintRight_toRightOf="parent"  
        app:layout_constraintTop_toTopOf="parent"  
        app:layout_constraintVertical_bias="0.073"  
        android:textAppearance="@style/Base.TextAppearance.AppCompat.Medium"/>  
  
    <Button  
        android:id="@+id/button1"  
        android:layout_width="95dp"  
        android:layout_height="wrap_content"  
        android:layout_marginBottom="8dp"  
        android:layout_marginEnd="8dp"  
        android:layout_marginStart="8dp"  
        android:layout_marginTop="8dp"  
        android:text="Button 1"  
        app:layout_constraintBottom_toBottomOf="parent"  
        app:layout_constraintEnd_toEndOf="parent"  
        app:layout_constraintHorizontal_bias="0.501"  
        app:layout_constraintStart_toStartOf="parent"  
        app:layout_constraintTop_toTopOf="parent"  
        app:layout_constraintVertical_bias="0.498" />  
  
    <Button  
        android:id="@+id/button2"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_marginBottom="80dp"  
        android:layout_marginEnd="8dp"  
        android:layout_marginStart="8dp"  
        android:layout_marginTop="8dp"  
        android:text="Button 2"  
        app:layout_constraintBottom_toBottomOf="parent"  
        app:layout_constraintEnd_toEndOf="parent"  
        app:layout_constraintStart_toStartOf="parent"  
        app:layout_constraintTop_toTopOf="parent"  
        app:layout_constraintVertical_bias="0.762" />  
  
    <Button  
        android:id="@+id/button3"  
        android:layout_width="101dp"  
        android:layout_height="48dp"  
        android:layout_marginBottom="8dp"  
        android:layout_marginEnd="8dp"  
        android:layout_marginStart="8dp"  
        android:layout_marginTop="8dp"  
        android:onClick="clickButton"  
        android:text="Button 3"  
        app:layout_constraintBottom_toBottomOf="parent"  
        app:layout_constraintEnd_toEndOf="parent"  
        app:layout_constraintHorizontal_bias="0.502"  
        app:layout_constraintStart_toStartOf="parent"  
        app:layout_constraintTop_toTopOf="parent"  
        app:layout_constraintVertical_bias="0.774" />  
</android.support.constraint.ConstraintLayout>  
```
### MainActivity.kt
Add the following code in the MainActivity.kt class. In this class, we implement the setOnClickListener listener on the button, implements the OnClickListener of View class (View.OnClickListener) and override its function onClick. In this class, we also create a Button programmatically (button4), define its properties and set it on the layout.
``` kotlin
package example.javatpoint.com.kotlinbutton  
  
import android.support.v7.app.AppCompatActivity  
import android.os.Bundle  
import android.view.View  
import android.view.ViewGroup  
import android.widget.Button  
import android.widget.Toast  
import kotlinx.android.synthetic.main.activity_main.*  
  
class MainActivity : AppCompatActivity() , View.OnClickListener {  
    val button4_Id: Int = 1111  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)  
        button1.setOnClickListener(){  
            Toast.makeText(this,"button 1 clicked", Toast.LENGTH_SHORT).show()  
        }  
        button2.setOnClickListener(this)  
        // add button dynamically  
        val button4 = Button(this)  
        button4.setText("Button 4 added dynamically")  
        button4.setLayoutParams(ViewGroup.LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT))  
        button4.setId(button4_Id)  
        button4.x = 250f  
        button4.y = 500f  
        button4.setOnClickListener(this)  
        constraintLayout.addView(button4)  
    }  
    override fun onClick(view: View) {  
        //  TODO("not implemented") //To change body of created functions use File | Settings | File Templates.  
        when (view.id) {  
            R.id.button2 ->  
                Toast.makeText(this,"button 2 clicked", Toast.LENGTH_SHORT).show()//single line code  
            button4_Id->{//multiline code  
                val myToast = Toast.makeText(this,"button 4 clicked", Toast.LENGTH_SHORT)  
                myToast.show()  
            }  
        }  
    }  
    fun clickButton(v: View){  
        val mToast = Toast.makeText(applicationContext,"button 3 clicked", Toast.LENGTH_SHORT)  
        mToast.show()  
    }  
}
``` 

[原文地址](https://www.javatpoint.com/kotlin-android-button)
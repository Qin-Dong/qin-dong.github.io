---
layout: post
title:  "Android读写资源文件"
categories: 安卓
tags: Android Java
author: QinDong
---
* content
{:toc}

### 一、 从resource中的raw文件夹中获取文件并读取数据（资源文件只能读不能写）

``` java
//在Activity上添加一个TextView并命名为myTextView，在res下新建一个raw文件夹，将资源文件放在raw下
TextView myTextView=(TextView)findViewById(R.id.myTextView);

String res = "";
try{
InputStream in = getResources().openRawResource(R.raw.bbi);
//在\Test\res\raw\bbi.txt,
   int length = in.available();
   byte [] buffer = new byte[length];
   in.read(buffer);
   //res = EncodingUtils.getString(buffer, "UTF-8");
   //res = EncodingUtils.getString(buffer, "UNICODE");
   res = EncodingUtils.getString(buffer, "BIG5");
   //依bbi.txt的编码类型选择合适的编码，如果不调整会乱码
   in.close();
   }catch(Exception e){
      e.printStackTrace();
   }
myTextView.setText(res);//把得到的内容显示在TextView上
```
 
### 二、 从asset中获取文件并读取数据（资源文件只能读不能写）

``` java
String fileName = "yan.txt"; //文件名字
String res="";
try{
   InputStream in = getResources().getAssets().open(fileName);
   // \Test\assets\yan.txt这里有这样的文件存在
   int length = in.available();
byte [] buffer = new byte[length];
in.read(buffer);
res = EncodingUtils.getString(buffer, "UTF-8");
}catch(Exception e){
      e.printStackTrace();
   }
```
 
### 三、 从sdcard中去读文件

首先要把文件通过\android-sdk-windows\tools\adb.exe把本地计算机上的文件copy到sdcard上去，adb.exe push e:/Y.txt /sdcard/, 不可以用adb.exe push e:\Y.txt \sdcard\ 同样： 把仿真器上的文件copy到本地计算机上用： adb pull ./data/data/com.tt/files/Test.txt e:/

``` java
String fileName = "/sdcard/Y.txt";
//也可以用String fileName = "mnt/sdcard/Y.txt";
String res="";
try{
FileInputStream fin = new FileInputStream(fileName);
//FileInputStream fin = openFileInput(fileName);
//用这个就不行了，必须用FileInputStream
    int length = fin.available();
    byte [] buffer = new byte[length];
    fin.read(buffer);
    res = EncodingUtils.getString(buffer, "UTF-8");
    fin.close();
    }catch(Exception e){
           e.printStackTrace();
}
myTextView.setText(res);
```
 
### 四、 写文件
一般写在\data\data\com.test\files\里面，打开DDMS查看file explorer是可以看到仿真器文件存放目录的结构的

``` java
   String fileName = "TEST.txt";
   String message = "FFFFFFF11111FFFFF" ;
writeFileData(fileName, message);
 
   public voidwriteFileData(String fileName,String message){
       try{
        FileOutputStream fout =openFileOutput(fileName, MODE_PRIVATE);
        byte [] bytes = message.getBytes();
        fout.write(bytes);
         fout.close();
        }
       catch(Exception e){
        e.printStackTrace();
       }
   }
```
 
### 五、 写， 读data/data/目录(相当AP工作目录)上的文件，用openFileOutput

``` java
   //写文件在./data/data/com.tt/files/下面
   public voidwriteFileData(String fileName,String message){
       try{
        FileOutputStream fout =openFileOutput(fileName, MODE_PRIVATE);
        byte [] bytes = message.getBytes();
        fout.write(bytes);
         fout.close();
        }
       catch(Exception e){
        e.printStackTrace();
       }
   }
//-------------------------------------------------------
//读文件在./data/data/com.tt/files/下面
   public String readFileData(String fileName){
        String res="";
        try{
         FileInputStream fin = openFileInput(fileName);
         int length = fin.available();
         byte [] buffer = new byte[length];
         fin.read(buffer);
         res = EncodingUtils.getString(buffer, "UTF-8");
         fin.close();
        }
        catch(Exception e){
         e.printStackTrace();
        }
        return res;
    }
```

### 六、 写， 读sdcard目录上的文件，要用FileOutputStream， 不能用openFileOutput

``` java
    //写在/mnt/sdcard/目录下面的文件 
   public voidwriteFileSdcard(String fileName,String message){
       try{
        //FileOutputStream fout = openFileOutput(fileName, MODE_PRIVATE);
       FileOutputStream fout = newFileOutputStream(fileName);
        byte [] bytes = message.getBytes();
        fout.write(bytes);
         fout.close();
        }
       catch(Exception e){
        e.printStackTrace();
       }
   }
 
   //读在/mnt/sdcard/目录下面的文件
   public String readFileSdcard(String fileName){
        String res="";
        try{
         FileInputStream fin = new FileInputStream(fileName);
         int length = fin.available();
         byte [] buffer = new byte[length];
         fin.read(buffer);
         res = EncodingUtils.getString(buffer, "UTF-8");
         fin.close();
        }
        catch(Exception e){
         e.printStackTrace();
        }
        return res;
   }
``` 

注： openFileOutput是在raw里编译过的，FileOutputStream是任何文件都可以
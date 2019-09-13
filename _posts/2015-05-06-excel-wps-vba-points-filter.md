---
layout: post
title:  "Excel或WPS中利用VBA编程进行地形点抽稀（过滤）"
categories: 测绘
tags: Excel WPS VBA 地形
author: QinDong
---
* content
{:toc}

工程测量工作中因计算工程量的需要，测量地形时不会考虑什么测图比例或出图比例，对计量有利的地形变化点都需要布点。这样会导致地形点过密，在出图时需要过滤掉一部分（不在纸质地形图上显示），毕竟纸质版资料还是要按规范要求的点间距来绘图的。在南方CASS中就有点过滤命令用于出图前的图面点过滤。我们在使用Civil3D做测量资料时，在出图方面存在一定局限性，如没有相应图式，也没有点过滤等命令。好在我们可以利用VBA进行扩展。

采用的方法是在Excel（WPS兼容）或是Civil3D里用VBA对地形数据文件过滤后生成新的数据文件，在Civil3D里新建一个曲面添加这个过滤后的点数据文件，出图时隐藏用于算量的曲面，将出图曲面进行绘图。
Excel里的代码：
![](/img/2015/2015-05-06-excel-wps-vba-points-filter-01.jpg)

在表格中添加一个按钮
![](/img/2015/2015-05-06-excel-wps-vba-points-filter-02.jpg)

``` vb
Private Declare Function ShellExecute Lib "Shell32.dll" Alias "ShellExecuteA" (ByVal hwnd As Long, ByVal lpOperation As String, ByVal lpFile As String, ByVal lpParameters As String, ByVal lpDirectory As String, ByVal nShowCmd As Long) As Long
Function Distance(Sx As Double, Sy As Double, Ex As Double, Ey As Double, Precision As Integer) As Double
Dim DltX As Double, DltY As Double
DltX = Ex - Sx
DltY = Ey - Sy
Distance = Round(Sqr(DltX * DltX + DltY * DltY), Precision)
End Function
Sub filter()
 Dim Dia1 As Object, Strr As String, PPath As String
 Dim filterDist As Integer '抽稀距离
 Dim Datums As Variant
 Dim RowIndex As Long
 Dim rIndex As Long, rIndex2 As Long, xa As Double, ya As Double, xb As Double, yb As Double
 RowIndex = 1
 filterDist = Sheet1.Cells(1, 6) '抽稀距离
 If filterDist = 0 Then filterDist = 2
 'Sheet1.Cells(1, 7) = "←F1单元格设置抽稀距离"
 
 'Sheet1.Cells(4, 7) = "cehui@139.com"
 Sheet1.Range("A1:E10000").ClearContents
 Set Dia1 = Application.FileDialog(msoFileDialogFilePicker)
 Dia1.Title = "版权所有(C) QQ:61902475 Email:cehui@139.com V20160225"
 With Dia1
 .AllowMultiSelect = False '限制只能同时选择一个文件
 .Filters.Clear
 .Filters.Add "南方CASS格式", "*.dat", 1 '限制显示的文件类型
 .Show
 For Each vrtSelectedItem In .SelectedItems
 PPath = vrtSelectedItem
 Next
 End With
 If Trim(PPath) <> "" Then
 Open PPath For Input As #1
 Do While Not EOF(1)
 Line Input #1, Strr
 If Trim(Strr) <> "" Then
 Datums = Split(Strr, ",")
 If UBound(Datums) = 4 Then
 Sheet1.Cells(RowIndex, 1) = RowIndex
 Sheet1.Cells(RowIndex, 2) = ""
 Sheet1.Cells(RowIndex, 3) = Datums(2)
 Sheet1.Cells(RowIndex, 4) = Datums(3)
 Sheet1.Cells(RowIndex, 5) = Datums(4)
 End If
 End If
 RowIndex = RowIndex + 1
 Loop
 Close #1
 End If
'点抽稀
 rIndex = 1
 rIndex2 = rIndex + 1
Do While Sheet1.Cells(rIndex, 1).Text <> ""
 If Trim(Sheet1.Cells(rIndex2, 2)) = "" Then
 xa = Sheet1.Cells(rIndex, 3)
 ya = Sheet1.Cells(rIndex, 4)
 rIndex2 = rIndex + 1
 Do While Sheet1.Cells(rIndex2, 1).Text <> ""
 If (Abs(Sheet1.Cells(rIndex2, 3).Text - xa) < filterDist And Abs(Sheet1.Cells(rIndex2, 4).Text - ya) < filterDist) Then
 If Distance(xa, ya, Sheet1.Cells(rIndex2, 3).Text, Sheet1.Cells(rIndex2, 4).Text, 3) < filterDist And Trim(Sheet1.Cells(rIndex, 2)) = "" And Trim(Sheet1.Cells(rIndex2, 2)) = "" Then
 Sheet1.Cells(rIndex2, 2) = "T"
 End If
 End If
 rIndex2 = rIndex2 + 1
 Loop
 
 End If
 rIndex = rIndex + 1
Loop
 If Trim(PPath) <> "" Then
 rIndex = 1
 RowIndex = 1
 Open Left(PPath, InStr(UCase(PPath), ".DAT") - 1) & "-抽稀(" & filterDist & "m)-" & Replace(Format(Date, "yyyy-mm-dd"), "-", "") & "-" & Replace(Time, ":", "") & ".dat" For Output As #2
 Do While Trim(Sheet1.Cells(rIndex, 1)) <> ""
 'Sheet1.Cells(5, 7) = rIndex & ":" & RowIndex
 If Trim(Sheet1.Cells(rIndex, 2)) = "" Then
 Print #2, RowIndex & ",," & Format(Sheet1.Cells(rIndex, 3), "0.000") & "," & Format(Sheet1.Cells(rIndex, 4), "0.000") & "," & Format(Sheet1.Cells(rIndex, 5), "0.000")
 RowIndex = RowIndex + 1
 End If
 rIndex = rIndex + 1
 Loop
 Close #2
 End If
Sheet1.Range("B1:B10000").ClearContents
End Sub
```

对于删除重合地形点同样有用，只要将过滤间距设为一个很小的值如0.1即可。

后续将介绍Civil3D里增加VBA点过滤功能，如下图：
点过滤对话框：
![](/img/2015/2015-05-06-excel-wps-vba-points-filter-03.jpg)
---
layout: post
title:  "利用VBA给Excel或WPS表格添加自定义函数（如：方位角函数）"
categories: 测绘
tags: Excel VBA WPS 方位角
author: QinDong
---
* content
{:toc}

在Excel或WPS表格中要使用VBA需要先打开“开发工具”菜单，打开方式请在网上度一下：
<div style="text-align:center;">![](/img/2019/20160516-custom-function-in-excel-wps-01.jpg)</div>
<div style="text-align:center;">在“自定义功能区”打开“开发工具”菜单</div>




点击“开发工具”菜单，在“开发工具”工具栏上点击“VB编辑器”：
<div style="text-align:center;">![](/img/2019/20160516-custom-function-in-excel-wps-02.jpg)</div>
<div style="text-align:center;">打开“VB编辑器”</div>

在“VB编辑器”内添加一模块文件（名称任意），在代码区添加函数代码：
<div style="text-align:center;">![](/img/2019/20160516-custom-function-in-excel-wps-03.jpg)</div>
<div style="text-align:center;">VB编辑器内添加模块并输入代码</div>

如添加一个测量方位角计算的函数，代码如下：

``` vb
Function Azimuth(Sx As Double, Sy As Double, Ex As Double, Ey As Double, Style As Integer)
Dim DltX As Double, DltY As Double, A_tmp As Double, Pi As Double
Pi = Atn(1) * 4 '定义PI值
DltX = Ex - Sx
DltY = Ey - Sy + 1E-20
A_tmp = Pi * (1 - Sgn(DltY) / 2) - Atn(DltX / DltY) '计算方位角
A_tmp = A_tmp * 180 / Pi '转换为360进制角度
Azimuth = Deg2DMS(A_tmp, Style)
End Function
```

这个函数中要用到一个格式转换函数：

``` vb
'转换十进制角度为度分秒
'Style=-1为弧度格式
'Style=0为“DD MM SS”格式
'Style=1为“DD-MM-SS”格式
'Style=2为“DD°MMˊSS""”格式
'Style=其它值时返回十进制度值
Function Deg2DMS(DegValue As Double, Style As Integer)
Dim tD As Integer, tM As Integer, tS As Double, tmp As Double, SignChar As String
If Sgn(DegValue) = -1 Then
SignChar = "-"
Else
SignChar = ""
End If
DegValue = Abs(DegValue)
tD = Fix(DegValue)
tmp = (DegValue - tD) * 60
tM = Fix(tmp)
tmp = (tmp - tM) * 60
tS = Round(tmp, 1)
'2016-12-27调整，避免出现60分、60秒的情况
If tS = 60 Then
tM = tM + 1
tS = 0
End If
If tM = 60 Then
tD = tD + 1
tD = 0
End If
Select Case Style
Case -1 '返回弧度
If SignChar = "-" Then
Deg2DMS = -DegValue * Atn(1) * 4 / 180
Else
Deg2DMS = DegValue * Atn(1) * 4 / 180
End If
Case 0
Deg2DMS = SignChar & tD & " " & Format(tM, "00") & " " & Format(tS, "00.0")
Case 1
Deg2DMS = SignChar & tD & "-" & Format(tM, "00") & "-" & Format(tS, "00.0")
Case 2
Deg2DMS = SignChar & tD & "°" & Format(tM, "00") & "ˊ" & Format(tS, "00.0") & """"
Case Else
If SignChar = "-" Then
Deg2DMS = -DegValue
Else
Deg2DMS = DegValue
End If
End Select
End Function
```

使用效果如Excel或WPS表格内部函数一样：
<div style="text-align:center;">![](/img/2019/20160516-custom-function-in-excel-wps-04.jpg)</div>
<div style="text-align:center;">输入函数前两个字母时会提示函数名</div>
<div style="text-align:center;">![](/img/2019/20160516-custom-function-in-excel-wps-05.jpg)</div>
<div style="text-align:center;">按提示输入值</div>
<div style="text-align:center;">![](/img/2019/20160516-custom-function-in-excel-wps-06.jpg)</div>
<div style="text-align:center;">计算结果</div>

将此表格保存或另存为模板即可，也可将模块文件导出，在其它表格中导入就可使用。
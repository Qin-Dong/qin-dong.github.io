---
layout: post
title:  "WPS表格快速批量打印测量成果表"
categories: 工程测量
tags: Excel WPS CASS VBA
author: QinDong
---
* content
{:toc}
### 程序要求

外业测量结束后成果数据从仪器下载后，需要立即打印成纸质版各方签字后存档。每次测量生成的数据或多或少，而完全靠人工排版不仅工作量巨大单调，且时间不允许。因此利用Excel或WPS宏进行批处理就显得非常必要。两河口水电站大坝标测量成果利用VBA宏完成测量成果的打印，效果较好。生成的表格如下图：




<img src="/img/2019/1568815966306.png" alt="1568815966306" style="zoom:80%;" />

生成的数据表打印预览：

![1568817863358](/img/2019/1568817863358.png)

当初在设置表格时所有参数均设置在VBA代码中，因近期将前往新的工地，此表格需作通用性调整。

> WPS表格中删除分页符的方法：选中分页符下一行第一列的单元格，在”页面布局“工具面板上的”插入分页符“工具列表中用”删除分页符“删除添加的分页符。自动产生的分页不能用此方法删除。

![1568817429343](/img/2019/1568817429343.png)

### 控制分面

以下代码在每页尾部自动添加一个分页符，以前的表格通过设置自动产生分页，在打印环境发生变化如不同的打印机和调整页眉、页脚后就会分页错位。按数据个数指定分页可有效避免分页错乱。

```vb
ActiveSheet.HPageBreaks.Add Before:=Range("A" & ((PageIndex + 1) * 50 + Start_Row)) '每一页尾添加分页符
```

### 固定表格

设置列宽代码：

```vb
Sub test()
	Sheet1.Range("a:b").ColumnWidth = 14 '设置A，B列为14像素宽
	Sheet1.Columns(3).ColumnWidth = 15 '设置c列为15像素宽
	Sheet1.Columns("d:e").ColumnWidth = 20 '设置d，e列为20像素宽
End Sub
```

表中代码：

```vb
    With ActiveSheet
        '设置列宽
        Columns("A").ColumnWidth = 5.6
        Columns("B").ColumnWidth = 12.13
        Columns("C").ColumnWidth = 11
        Columns("D").ColumnWidth = 8.2
        Columns("E").ColumnWidth = 5.6
        Columns("F").ColumnWidth = 12.13
        Columns("G").ColumnWidth = 11
        Columns("H").ColumnWidth = 8.2
                
        '设计头部行高
        Rows(1).RowHeight = 14.25
        Rows(2).RowHeight = 22.5
        Rows(3).RowHeight = 14.25
        Rows(4).RowHeight = 15
        Rows(5).RowHeight = 15
        Rows("6:1000").RowHeight = 13
    End With
```

### 设置页面格式

```vb
    With ActiveSheet.PageSetup
        .LeftHeader = ""
        .CenterHeader = ""
        .RightHeader = ""
        .LeftFooter = ""
        .CenterFooter = "&10测  量:                                  测量监理:                              测量中心:" & Chr(10) & "　　                                  " & Chr(10) & "第&P页，共&N页"
        .RightFooter = ""
        .Orientation = xlPortrait
        .Zoom = 100
        .FirstPageNumber = True
        .LeftMargin = 2.1 * lenUnitFactor '59.527559 2.1cm
        .RightMargin = 2.1 * lenUnitFactor '59.527559 2.1cm
        .TopMargin = 0.9 * lenUnitFactor '25.511811 0.9cm
        .BottomMargin = 1.1 * lenUnitFactor '31.181102 1.1cm
        .HeaderMargin = 0.7 * lenUnitFactor '19.84252 0.7cm
        .FooterMargin = 0.7 * lenUnitFactor '19.84252 0.7cm
        .CenterHorizontally = False
        .CenterVertically = False
        .PrintErrors = xlPrintErrorsDisplayed
        .Order = xlDownThenOver
        .PrintGridlines = False
        .PrintHeadings = False
        .BlackAndWhite = False
        .PrintQuality = 600
        .PaperSize = xlPaperA4
'        .PrintComments = -4142
'        .PrintArea = "$A$5:$H$205"
        .PrintTitleRows = "$1:$5"
        .PrintTitleColumns = "$A:$H"
    End With
```







未完待续。。。
---
layout: post
title:  "德州仪器TI nspire CAS工程测量程序"
categories: 计算器 测绘
tags: TI 工程测量
author: QinDong
---
* content
{:toc}

## 1、高斯坐标转工程坐标系

``` vb
Define a_gauss2gc()=
Prgm
setgeomode()
RequestStr "施工坐标系名=",tconame
0→stat
For i,1,dim(coname),1
 If coname[i]=tconame Then
 1→stat
 Request "待转换点X(N)=",tpx
 Request "待转换点Y(E)=",tpy
 xa[i]→txa
 ya[i]→tya
 xb[i]→txb
 yb[i]→tyb
 sa[i]→tsa
 R►Pθ(txb-txa,tyb-tya)→talpha
 EndIf
EndFor
If stat=1 Then
(tpx-txa).cos(talpha)+(tpy-tya).sin(talpha)+tsa→tstage
-(tpx-txa).sin(talpha)+(tpy-tya).cos(talpha)→toff
Disp "高斯坐标→施工坐标：",tconame
Disp "(桩号)x=",round(tstage,3)
Disp "(偏距)y=",round(roff,3)
Else
Disp "选择的坐标系未定义！"
EndIf
EndPrgm
``` 
 
### 2、工程坐标转高斯坐标系

``` vb
Define a_gc2gauss()=
Prgm
Setgeomode()
RequestStr  “施工坐标系名=”,tconame
0→stat
For i,1,dim(coname),1
 If  coname[i]=tconame Then
 1→stat
 Request “待转换点桩号x=”,tpx
 Request “待转换点偏距y=”,tpy
 xa[i]→txa
 ya[i]→tya
xb[i]→txb
yb[i]→tyb
sa[i]→tsa
R►Pθ (txb-txa,tyb-tya)→talpha
EndIf
EndFor
If stat=1 Then
txa+(tpx-tsa).cos(talpha)-tpy.sim(talpha)→tpn
tya+(tpx-tsa).sin(talpha)+tpy.cos(talpha)→tpe
Disp “施工坐标→高斯坐标 坐标系:”,tconame
Disp “X(N)=”,round(tpn,3)
Disp “Y(E)=”,round(tpe,3)
Else
Disp “选择的坐标系未定义！”
EndIf
EndPrgm
```
 
### 3、setgeomode子程序

``` vb
Define setgeomode()=
Prgm
If getMode(2)≠2 Then
setMode(1,1)
setMode(2,2)
setMode(5,2)
setMode(7,1)
setMode(8,1)
EndIf
EndPrgm
```
 
### 4、工程坐标系转工程坐标系

``` vb
Define a_gc2gc()=
Prgm
RequestStr “源施工坐标系名=”,tconame
0→stat
For i,1,dim(coname),1
 If coname[i]=tconame Then
 1→stat
 Request “待转换点桩号x=”,tpx
 Request “待转换点偏距y=”,tpy
 xa[i]→txa
 ya[i]→tya
 xb[i]→txb
 yb[i]→tyb
 sa[i]→tsa
 R►Pθ(txb-txa,tyb-tya)→talpha
 EndIf
EndFor
If stat=1 Then
txa+(tpx-tsa).cos(talpha)-tpy.sin(talpha)→tpn
tya+(tpx-tsa).sin(talpha)+tpy.cos(talpha)→tpe
RequestStr “目标坐标系名=”,tarconame
0→stat2
For i,1,dim(coname),1
 If coname[i]=tarconame Then
 1→stat2
 xa[i]→txa
 ya[i]→tya
 xb[i]→txb
 yb[i]→tyb
 sa[i]→tsa
 R►Pθ(txb-txa,tyb-tya)→talpha
 EndIf
EndFor
If stat2=1 Then
(tpn-txa).cos(talpha)+(tpe-tya).sin(talpha)+tsa→tstage
-(tpn-txa).sin(talpha)+(tpe-tya).cos(talpha)→toff
Disp “施工坐标系间转换:”,tconame & “→”& tarconame
Disp “（桩号）x=”,round(tstage,3)
Disp “(偏距) y=”,round(roff,3)
Else
Disp “选择的目标坐标系未定义！”
EndIf
Else
Disp “选择的源坐标系未定义！”
EndIf
EndPrgm
```
 
### 5、坐标反算
``` vb
Define a_zbfs()=
Prgm
setgeomode()
Request “起点X=”,x1
Request “起点Y=”,y1
Request “终点X=”,x2
Request “终点Y=”,y2
Disp “距离=”,R►Pr(x2-x1,y2-y1)
R►Pθ (x2-x1,y2-y1)→α
If α<0 Then
α+360→α
EndIf
Disp “方位角=”,α►DMS
EndPrgm
```
 
### 6、坐标正算
``` vb
Define a_zbzs()=
Prgm
setgeomode()
Request “起点X=”,x1
Request “起点Y=”,y1
Request “距离=”,s
RequestStr “方位角(D.MMSS)=”,str
expr(str)→α
fdms(α)→α
Disp “计算点X=”,round(x1+s.cos(α),3)
Disp “计算点Y=”,round(y1+s.sin(α),3)
EndPrgm
 
7、转换D.MMSS格式为十进制度
Define fdms(α)=
Func
Return intDiv(α,1)+(intDiv(remain(α,1).100,1))/60+(remain(α*100,1).100)/3600
EndFunc
```

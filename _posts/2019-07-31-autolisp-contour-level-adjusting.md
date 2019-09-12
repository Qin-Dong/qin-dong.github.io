---
layout: post
title:  "AutoLISP工具-快速更正等高线高程"
categories: AutoLISP 测绘
tags: 等高线 AutoLISP
author: QinDong
---
* content
{:toc}

密瓦IX标县道工程共11公里余。目前获得前期带状地形图为多幅拼接，DWG格式。图中等高线属性标高与等高线值不相符合，经检查，每幅图中在出图时将所有等高线作了升高或降低处理。但在同一幅图内，差值是常数。前几日在处理第一段时手工操作，费时费力，且容易出错。在生成曲面时才发现有极个别很小的等高线未更正，导致曲面生成后出现极高、极低点。因后续尚有9公里的带状地形等高线需要更正，工作量巨大，必须借用LISP进行批量修改。程序如下：

``` autolisp
;2019-08-06增加了等高线为二维多义线的处理办法
 
(defun c:zzElevCorrecting ()		;程序名:zzElevCorrecting，加载后在命令行输入。
  (setq elevDiff (getreal "请输入等高线的高程差值: "))
  (setq elevDist (getint "请输入等高距: "))
  (prompt "\n <<框选欲更改高程的等高线>>")
  (setq ss (ssget))			;取得选择集。
  (setq	n 0
	Cnt 0
  )					;选择集的起始值n＝0
  (repeat (sslength ss)			;计算选择集的对象个数。
    (setq en (ssname ss n))		;依据索引值取出选择集中的图元名。
    (setq endata (entget en))		;取得对象的联合列表。
    (setq oldColor (cdr (assoc 62 endata)))
    (setq entType (cdr (assoc 0 endata)))
    (setq oldLayer (cdr (assoc 8 endata)))
    (if	(and (/= oldLayer "1-地形-等高线-更正") ;等高线为多义线时的处理
	     (= entType "LWPOLYLINE")
	)
      (progn
	(setq elePl (cdr (assoc 38 endata)))
					;38表示线段的高程,elePl:多义线标高
	(setq elePlProp (assoc 38 endata)) ;elePlProp:多义线标高原属性
	(setq eleRt (+ elePl elevDiff))	;多义线正确的高程值，其标高加上高差
	(setq eleRtProp (cons 38 eleRt));eleRtProp:加上高差改正后正确有多义经标高属性表
 
	(if (= (rem eleRt (* 5 elevDist)) 0)
	  (setq newColor 1)		;计曲线红色
	  (setq newColor 2)		;首曲线黄色
	)
	(setq endata (subst eleRtProp elePlProp endata))
					;用新的属性表更改旧的属性表
					;替代成为新的关系属性
					;(setq oldLayer (assoc 8 endata))	;找到原图层
	(setq newLayer (cons 8 "1-地形-等高线-更正")) ;建立新的图层
	(setq endata (subst newLayer (cons 8 oldLayer) endata))
					;把赋了高程值的等高线放在新的图层中。
	(setq
	  endata (subst (cons 62 newColor) (cons 62 oldColor) endata)
	)
	(entmod endata)			;按更新的属性列表新生成屏幕上的对象
	(setq Cnt (1+ Cnt))
      )					;progn
    )					;if
 
    (if	(and (/= oldLayer "1-地形-等高线-更正")
	     (= entType "POLYLINE")	;当等高线为二维多义线的处理
	)
      (progn
	(setq elePl (last (assoc 10 endata)))
	(print elePl)			;38表示线段的高程,elePl:多义线标高
	(setq elePlProp (assoc 10 endata)) ;elePlProp:多义线标高原属性
	(setq eleRt (+ elePl elevDiff))	;多义线正确的高程值，其标高加上高差
	(setq eleRtProp (list 10 0.0 0.0 eleRt))
					;eleRtProp:加上高差改正后正确有多义经标高属性表
 
	(if (= (rem eleRt (* 5 elevDist)) 0)
	  (setq newColor 1)		;计曲线红色
	  (setq newColor 2)		;首曲线黄色
	)
	(setq endata (subst eleRtProp elePlProp endata))
					;用新的属性表更改旧的属性表
					;替代成为新的关系属性
					;(setq oldLayer (assoc 8 endata))	;找到原图层
	(setq newLayer (cons 8 "1-地形-等高线-更正")) ;建立新的图层
	(setq endata (subst newLayer (cons 8 oldLayer) endata))
					;把赋了高程值的等高线放在新的图层中。
	(setq
	  endata (subst (cons 62 newColor) (cons 62 oldColor) endata)
	)
	(entmod endata)			;按更新的属性列表新生成屏幕上的对象
	(setq Cnt (1+ Cnt))
      )					;progn
    )					;if
 
 
    (setq n (1+ n))
  )
  (princ
    (strcat "\n 共有< " (itoa Cnt) " >条等高线更新完毕！")
  )
  (prompt "快速更正等高线高程。(C)QinDong 2019.07.31 密瓦IX标"
  )
  (prin1)
)
```

``` commonlisp
    (strcat "\n 共有< " (itoa Cnt) " >条等高线更新完毕！")
```

``` visuallisp
    (strcat "\n 共有< " (itoa Cnt) " >条等高线更新完毕！")
```


有以下原地形图：

![](/img/2019/2019073110101-autolisp-contour01.png)

更正后地形图：

![](/img/2019/2019073110101-autolisp-contour02.png)

更正后将计曲线设为红色、首曲线设为黄色，更新至新层，以后执行时会自动跳过已处理过的等高线。
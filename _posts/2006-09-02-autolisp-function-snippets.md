---
layout: post
title:  "AutoLISP笔记-常用函数代码片断"
categories: AutoLISP
tags: AutoLISP 字符串
author: QinDong
---
* content
{:toc}

### 1、字符串按指定分隔符分隔

```lisp
;;;功能：字符串按指定分隔符分隔
;;;(split "1,,255280.791,3743764.732,2786.206" ",")
;;;返回：("1" "" "255280.791" "3743764.732" "2786.206")
(defun split (str delim / LST POS)
  (while (setq pos (vl-string-search delim str))
    (setq lst (append lst (list (substr str 1 pos))))
    (setq str (substr str (+ 2 pos)))
  )
  (if (> (strlen str) 0)
    (append lst (list str))
    lst
  )
)
```

### 2、字符串按指定分隔符分隔增强

分隔符可以是字串，用于取文件名，用扩展名作为分隔符

```commonlisp
;;;(splitX "C:\\Users\\....25～K1+013.52；EL.2776.73～EL.2804.74）.dat" ".dat")
;;;返回：(C:\\Users\\....25～K1+013.52；EL.2776.73～EL.2804.74）)
(defun splitX (str delim / LST POS)
  (while (setq pos (vl-string-search delim str))
    (setq lst (append lst (list (substr str 1 pos))))
    (setq str (substr str (+ (+ pos (strlen delim)) 1)))
  )
  (if (> (strlen str) 0)
    (append lst (list str))
    lst
  )
)
```

### 3、获取日期

```commonlisp
;;;功能：获取时间，要用到splitx自定义函数，返回“20170511”类型的字符串
(defun date ()
  (setq datetime (rtos (getvar "cdate") 2 6))
  (car (splitx datetime "."))
)
```

### 4、获取日间

```commonlisp
;;;功能：获取时间函数，要用到splitx自定义函数，返回“110253”类型的时间字符串
(defun time ()
  (setq datetime (rtos (getvar "cdate") 2 6))
  (cadr (splitx datetime "."))
)
```

### 5、指定点写入文本串

```commonlisp
;功能：在指定点写入文本串
;用法：(_text (getpoint "Pick") "3333" 2 0)
(defun _text (point string height rotation / space text)
  (setq	acdoc (vla-get-activedocument (vlax-get-acad-object))
		space (vlax-get-property
		acdoc
				(if (= 1 (getvar 'CVPORT))
				  'Paperspace
				  'Modelspace
				)
		      )
  )
  (setq text (vla-addtext space string (vlax-3D-point point) height))
  (vla-put-alignment text acalignmentmiddlecenter)
  (vla-put-textalignmentpoint text (vlax-3D-point point))
  (vla-put-rotation text rotation)
  text
)
```

### 6、用系统关联软件打开文件

```commonlisp
;用法：(setq fl (getfiled "创建面积统计文件" (cond ( \*file\* ) ( "" )) "txt;csv;xls" 1))
;(_Open (findfile fl))
  (defun _Open ( target / Shell result )
    (if (setq Shell (vla-getInterfaceObject (vlax-get-acad-object) "Shell.Application"))
      (progn
        (setq result
          (and (or (eq 'INT (type target)) (setq target (findfile target)))
            (not
              (vl-catch-all-error-p
                (vl-catch-all-apply 'vlax-invoke (list Shell 'Open target))
              )
            )
          )
        )
        (vlax-release-object Shell)
      )
    )
    result
  )
```


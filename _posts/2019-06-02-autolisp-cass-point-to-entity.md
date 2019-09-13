---
layout: post
title:  "AutoCAD Civil 3D中将CASS地形图中地形点转换成CAD点实体"
categories: Civil3D AutoCAD 测绘 AutoLISP
tags: AutoCAD Civil3D CASS AutoLISP
author: QinDong
---
* content
{:toc}

使用AutoCAD Civil 3D打开CASS生成的地形图，由于CASS地形图中的地形点是以块形式存在的，虽然我们可以使用CAD的图元对象中的块进行曲面定义，但这样做的结果就是虽有曲面但没有对应的地形点。我们需要将块状地形点转换成CAD的点对象，再在AutoCAD Civil 3D里根据CAD点生成Civil 3D格式的点对象，并加入点编组用于后组定义曲面或导出等操作。

以下代码的使用方法是，用AutoCAD Civil 3D打开CASS地形图，在命令行执行代码中的命令即可，图中所有块状地形点将消失，取而代之的是生成一些CAD点。

```
(defun C:CASS2POINT ()
;  (setq ss (ssget "X" '((0 . "insert") (8 . "GCD"))))
  (setq ss (ssget "X" '((0 . "insert") (2 . "GC*"))))
  (setq i 0)
  (repeat (sslength ss)
    (setq ssn (ssname ss i))
    (setq endata (entget ssn))
    (setq blockXYZ (assoc 10 endata))
    (print blockXYZ)
    (entmakeX (list '(0 . "POINT") blockXYZ))
    (setq i (1+ i))
    (entdel ssn)
  )
)
```

![](/img/2019/201906020101.png)

![](/img/2019/201906020102.png)

![](/img/2019/201906020103.png)
---
layout: post
title:  "Visual Studio Code之Markdown编辑器增强"
categories: 工具软件
tags: VisualStudio IDE Markdown
author: QinDong
---
* content
{:toc}

一直想找一个功能齐全的Markdown编辑器，最好是能粘贴图片并自动将图片保存到指定目录且在文中添加图片地址。经过耐心的寻找、安装试用，发现“Typora”非常不错。此软件对粘贴图片支持较好，而且是同步渲染预览。但在安装Visual Studio Code后在扩展插件的支持下，也能实现相同功能，而且VScode功能更多，界面更美观，非常满意。现就VScode完善Markdown编辑器功能进行的设置详述如下：




Paste Image是一个截图粘贴的插件。用截图软件截图后，<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>v</kbd>粘贴到markdown文档上。

### 安装扩展Paste Image

![20190920-201140](/img/2019/20190920-201140.png)

### 设置Paste Image参数

需要设置的主要参数为：

- Default Name：YMMDD-HHmmss
- Paste Image: Path 粘贴文件时存放图片的目标目录
- Paste Image: Base Path 这个设置会影响到粘贴文件时候 (/image/xxxx.png) 路径的粘贴, 不然会显示 (../images/xxxx.png)
- Paste Image: Insert Pattern 粘贴到文档中的图片URI


Jekyll博客中的图片不能放在文档所在目录下并在Markdown中使用相对目录，这样在上传后图片不能显示。必须存放在博客根目录下的一个子目录中如“img”，并在文档中图片地址以“/img/”格式从根开始，若使用相对地址，会出现在首页显示图片而进入文档后又不显示的情况，这是因在进入文档后地址会相对地发生改变。

针对此情况，图片只能存放在“_post”同级的“img”目录下，对此设置为：

```
- Default Name：“YMMDD-HHmmss”
- Paste Image: Path “../img/2019/”
- Paste Image: Base Path “/img/2019/”
- Paste Image: Insert Pattern “![${imageFileNameWithoutExt}](/img/2019/${imageFileName})”
```

参数设置见下图：

![20190920-201449](/img/2019/20190920-201449.png)

![20190920-201653](/img/2019/20190920-201653.png)

其它参数保持默认即可。

Paste Image安装设置好后，用截图软件截图到剪贴板，回到VS code中用快捷键<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>V</kbd>即可将图片保存到文档下的“Images”目录下并同时将图片地址添加到文档中。

唯一的遗憾是预览时不能显示图片，只能显示一个无效提示。


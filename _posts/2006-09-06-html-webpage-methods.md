---
layout: post
title:  "网页技巧集锦"
categories: 网页
tags: HTML CSS JavaScript
author: QinDong
---
* content
{:toc}

在进行博客页面定制时，原主题的部分做得不够精细，如文章列表中文章标题过长会出与换行，影响列表美观，就需要限定列表中每行文本的长度，将超过指定字数的文本截断用省略号代替。本人以前管理个人网站时经常进行类似网页处理，现将网页编辑的一些技巧罗列如下：

### 网页中单行文限长

``` HTML
<style>
  .container{
    display: inline-block;
    vertical-align:middle;
    width:120px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
 }
</style>
```

网页中如下使用：

``` HTML
<ul>
<li> <a  href="https://www.baidu.com/" target="_blank"><b class="container">列表中本行文本超过一定长度按指定长度+...</b></a>
</li>
</ul>
```

以上代码在本博客页中如下使用：

``` HTML
                <ul class="content-ul" recent>
                    {% for post in site.posts offset: 0 limit: 10  %}
                        <li><div style="display: inline-block;vertical-align: middle;width: 180px;white-space: nowrap;overflow: hidden;text-overflow: ellipsis;"><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></div></li>
                    {% endfor %}
                </ul>
```

效果比较理想，但网文介绍上述方法只在chrome及相同内核浏览器上生效，其它浏览器上可能不能实现预期功能。遇此情况可能需要用后述js方法实现。

### js实现单行限长

``` HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        #container {
            height: 50px;
            line-height: 25px;
            overflow: hidden;
            width:100px;
            border:1px solid black;
        }

        #text{
            margin:0px;
            padding:0px;
        }
    </style>
</head>

<body>
<div id="container">
    <p id="text">
        列表中本行文本超过一定长度按指定长度+...</p>
</div>

<script>
        var divH = document.getElementById("container").clientHeight;
        var p = document.getElementById("text");
        while (p.clientHeight > divH) {
            p.innerText = p.innerText.replace(/(\s)*([a-zA-Z0-9]+|\W)(\.\.\.)?$/, "...");
        };
</script>

</body>
</html>
```

可借鉴上述用JS替换指定标签文本的方法实现对网页中的内容进行编辑。
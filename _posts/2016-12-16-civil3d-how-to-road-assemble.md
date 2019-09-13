---
layout: post
title:  "如何Civil3D软件来装配道路"
categories: Civil3D AutoCAD 测绘
tags: Civil3D AutoCAD 道路 装配 部件
author: QinDong
---
* content
{:toc}

### 基准线
Civil 3D喜欢以路的中心线作为道路的基准线。所以你最好有这样一条中心线。如果万一你没有，你可以用这个功能生成中心线——在标注菜单栏里：中心线。
![](/img/2018/20161216-civil3d-how-to-road-assemble-01.jpg)
现在默认你已经画好了中心线。现在去home栏，找到基准线（alignment，我不是很清楚中文版的叫啥）——从实体创建基准线——选择已经画好的那根中心线——会出来一个箭头让你确定方向（开车的方向），如果对就回车，不对就选翻转——起个名字，线的类型选设计，去掉勾选，搞定。




![](/img/2018/20161216-civil3d-how-to-road-assemble-02.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-03.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-04.jpg)

你现在得到了一根绿色的基准线。水平面的维度有了，还需要一个立面的维度，这个时候我们之前生成那个路面就派上用场了，那个面上每个点都是有高程的。因此基准线和那个路面的交线，就是我们想要的横断面上的剖面。

具体就是，点击你画好的绿线，一个新的菜单蹦出来，选择生成高程——点你设计的路面——添加——点击在横断面里创建——窗口消失了， 你找个空白的地方点击鼠标左键——出现了。

![](/img/2018/20161216-civil3d-how-to-road-assemble-05.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-06.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-07.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-08.jpg)

看起来还不错，横坐标那里是链测长度，只到240米，因为在240米以后，这个路多了一条支路并入，而且路宽变了，那个地方还要重新画。但是这样的话可能会画不止一个横断面，麻烦。所以干脆把这个表画大一点，剩下的路也装进来。我们来改一下这个表的值域。

选中表，在横断面属性那里选择编辑横断面属性，分别在站和高程选项卡里选择自定义，搞定。

![](/img/2018/20161216-civil3d-how-to-road-assemble-09.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-10.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-11.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-12.jpg)

变大了。

但是这个只是中心线的高程，我们还有两个路缘的高程——已经被设计好的——需要被添加进来。就去之前的那个设计图里，把它们复制粘贴过来就好了。

![](/img/2018/20161216-civil3d-how-to-road-assemble-13.jpg)

深蓝色是道路行驶方向的右侧，也是外侧；浅蓝色是内侧。

但是呢，这两个根线是AutoCAD里复制过来的，对于civil 3d没有任何意义。我们需要把这些polyline 转化成 profile line (剖面线)，正常的方法是用这个工具一点一点画。

![](/img/2018/20161216-civil3d-how-to-road-assemble-14.jpg)

但是，作为一个能偷懒绝不费力的懒癌患者，我是绝对不会一点一点画这种东西的。

因此在这里介绍一个黑科技：装一个叫 ProfileTollBox.dll 的插件。

命令栏里输入netload, 上载该插件，然后在命令栏里输入PFP, 先选择要转化的polyline, 再选择所在的视图，Nailed it！

![](/img/2018/20161216-civil3d-how-to-road-assemble-15.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-16.jpg)

### 部件

进入下一步，现在该有的生产流水线都有了，再设计一个装配零件，顺着流水线走下去，就能生成一条实在的路了。

装配零件就是路的剖面设计。

civil 3D立面有个一个装配零件库，在视图那里打开工具盘就能看到。

![](/img/2018/20161216-civil3d-how-to-road-assemble-17.jpg)

就是左面这个东西，这里面有好多好多各种类型已经建模好的装配件，如果不能满足你的需求，还可以用一个叫做Subassembly Composer的官方插件自定义。今天先用不到。

![](/img/2018/20161216-civil3d-how-to-road-assemble-18.jpg)

总之，我们再看下这个路到底是怎么设计的。

![](/img/2018/20161216-civil3d-how-to-road-assemble-19.jpg)

![](/img/2018/20161216-civil3d-how-to-road-assemble-20.jpg)

现在知道了，是双车道，两侧有路肩。所以就以基准线的起点为原点，新建一个装配。样式选可共面路，代码选所有代码。点确定。

![](/img/2018/20161216-civil3d-how-to-road-assemble-21.jpg)

在空白处点一下，出来一个装配中心，该中心是以路中央的基准线为准。

![](/img/2018/20161216-civil3d-how-to-road-assemble-22.jpg)

在工具盘里选择车道，选LaneSuperelevationAOR。 为什么选这个呢，因为这个装配设计了对齐目标，可以之前提到的路缘的高程进行对齐。至于其他的装配怎么用，可以右键配件点帮助，会跳出来一个页面解释。

![](/img/2018/20161216-civil3d-how-to-road-assemble-23.jpg)

总之选好了装配以后对装配中心点两下，就出来两条车道。但是两条车道倾斜角度不对，需要调整。

![](/img/2018/20161216-civil3d-how-to-road-assemble-24.jpg)

选中左面那条，按照设计要求更改宽度为3.7，倾斜角正的，等等。右侧同理。注意这里其实设计图没有给倾斜角度。因为整条路的倾斜角度不是常数。这个时候选择这个装配的重要性就体现出来了，因为可以让车道两端分别和路缘的设计高程对齐，这样倾斜角会自动更改。所以这里先采用默认值，调整好方向就可以。

![](/img/2018/20161216-civil3d-how-to-road-assemble-25.jpg)

总之做好了以后长这样，和设计图还是比较接近的。

![](/img/2018/20161216-civil3d-how-to-road-assemble-26.jpg)

### 装配

接下来开始进行装配，新建一个走廊（corridor）。

![](/img/2018/20161216-civil3d-how-to-road-assemble-27.jpg)

装配点将沿着水平基准线和竖直高程线前进，装配部件也据此装配。因此要选择好。

![](/img/2018/20161216-civil3d-how-to-road-assemble-28.jpg)

新的窗口跳出来，选择设定所有目标。在高程一栏给左右路缘分别选择合适的高程，确定。

![](/img/2018/20161216-civil3d-how-to-road-assemble-29.jpg)

然后给它添加表面。在表面一栏里左边点一个按钮，添加表面。

![](/img/2018/20161216-civil3d-how-to-road-assemble-30.jpg)

然后给它定义一下渲染材料。

![](/img/2018/20161216-civil3d-how-to-road-assemble-31.jpg)

在代码里选择顶点，右面加号添加。

![](/img/2018/20161216-civil3d-how-to-road-assemble-32.jpg)

再设置一下路的表面的边界线。右键单击，选第一个，以走廊外线作为外边界。

![](/img/2018/20161216-civil3d-how-to-road-assemble-33.jpg)

点确定。看一下效果。

![](/img/2018/20161216-civil3d-how-to-road-assemble-34.jpg) 
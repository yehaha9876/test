[CSS框架-960 Grid System](http://www.cnblogs.com/hankering/articles/1753263.html)
=================================================================================

**CSS框架 ：960 Grid System  官网：****[http://960.gs/](http://960.gs/)**

 

什么是框架？框架是一种你能够使用在你的web项目中概念上的结构。CSS框架一般是CSS文件的集合，包括基本风格的字体排版，表单样式，表格布局等等，比如：

 

    \* typography.css 字体排版规则
    \* grid.css 表格布局
    \* layout.css 布局
    \* form.css 表单
    \* general.css CSS常规设置

下面是一些不错的CSS框架，推荐。

### [Blueprint CSS](http://code.google.com/p/blueprintcss/)

Blueprint是一个CSS框架，它的目标是减少你的CSS开发时间。它提供给你强大的CSS基础来创建你的项目，包括易于使用的grid，有效的字体排版，以及可打印的stylesheet .

 

### [![Elements](/pictures/)](http://960.gs/)

### [960 Grid System](http://960.gs/)

 

 

 

 **一、960的奥妙**

      从数学着手： 960可以分解为2的6次方乘以3和5, 这使得960可以分割成以下宽度的整数倍：

    2, 3, 4, 5, 6, 8, 10, 12, 15, 16, 20, 24, 30, 32, 40,48, 60, 64, 80, 96, 120, 160, 192, 240, 320, 480

共26种（26 = 7 \* 2 \* 2 – 2, 减去2是去掉1和960自身），我们标记为：

    N(960) = N(2^6 * 3 * 5) = 26

根据上面的算法，可以得到：

    N(360) = N(2^3 * 3^2 * 5) = 22N(480) = N(2^5 * 3 * 5) = 22N(720) = N(2^4 * 3^2 * 5) = 28N(750) = N(2 * 3 * 5^3) = 14N(800) = N(2^5 * 5^2) = 16N(960) = N(2^6 * 3 * 5) = 26N(1000) = N(2^3 * 5^3) = 14N(1024) = N(2^10) = 9N(1440) = N(2^6 * 3^2 * 5) = 34N(1920) = N(2^7 * 3 * 5) = 30

从上述算法我们可以得出以下结论：

要使得N(width)最大，width的取值有两个系列： A系列： …, 320, 720, 1440, … B系列： …, 480, 960, 1920, … N越大，可组合的宽度值就越多。

      目前绝大多数显示器都支持 1024 x 768 及其以上分辨率，而960恰好是1024 x 768 分辨率下最灵活也是最合适的尺寸，这些条件决定了960成了目前设计中最完美的尺寸。（PS：此部分内容大多摘自：[960 Grid System 研究](http://www.fooldy.cn/?p=878)）

 

**二、960网格系统简介**

 

**本质：**

[960 Grid System](http://960.gs/) 是一套基于宽度为 960px CSS 框架，它为网页布局提供了通用的尺寸设置，它提供了两种不同的尺寸布局：12列和16列，它们可以独立使用，也可以一起使用。

**尺寸：**

12列的布局，将总宽分成12份，每份的宽度是60px，而16列的布局分成16份，每份的宽度是40px，每部分左右边距都是10px，从而每列产生20px的空隙。

**目的：**

该系统以快速原型开发为出发点，但同时也能很好地集成到生产环境中去。它同时提供了打印布局、设计布局和一个能提供一致尺寸的 CSS 文件。

 

 [960 Grid System](http://960.gs/)是一个非常棒的布局辅助设计系统，以960PX为基准宽度提供了12列和16列两种布局模式（PS：目前官方已经提供了[24列的布局模式](http://960.gs/demo_24_col.html)，相信作者以后还会提供更多的布局模式）。

      **1：alpha与omega**

      960 Grid System的布局宽度为960PX，但由于每列左右均由10PX的margin（外补丁），因此内容宽度实际为940px。

    /*alpha：用于清除左边10px的marginomega：用于清除右边10px的margin*/

      PS：从作者取alpha与omega这两个名字可以想象一下或许作者很热爱希腊文化。alpha（α）在*希腊字母*表里，它是第一个字母，读作“阿尔法”（阿拉法），代表开始；omega（γ）在*希腊字母*表里，它是最后一个字母，读作“欧美噶”（俄梅嘎），代表终了。

     **2：prefix\_XX与suffix\_xx**

    /*用于在每个单元网格的前面或后面添加空白的列（栏）*//*用法：<div class="prefix_15 grid_1">IT北瓜</div>*/

      **3：clear与hr**

    /*用于清除层的浮动*//*用法：<div class="clear"></div>*//*但假如你查看过官网主页的源代码你会发现，作者用的是<hr />*//*用法：可参考官网主页的源代码<hr />*/

     **4：push\_xx与pull\_xx**

    /*这是2009-06-29更新新增的类，用于重新定制布局顺序*//*用法：引用官网主页源代码*/<h1 class="grid_4 push_4">    960 Grid System</h1><!-- end .grid_4.push_4 --><p id="description" class="grid_4 pull_4">    <a href="files/960_download.zip">Download</a> &larr; Templates for <a href="http://www.adobe.com/products/fireworks/">Fireworks</a>, <a href="http://www.adobe.com/products/indesign/">InDesign</a>, <a href="http://www.inkscape.org/">Inkscape</a>, <a href="http://www.adobe.com/products/illustrator/">Illustrator</a>, <a href="http://www.omnigroup.com/applications/omnigraffle/">OmniGraffle</a>, <a href="http://www.adobe.com/products/photoshop/">Photoshop</a>, <a href="http://office.microsoft.com/en-us/visio/default.aspx">Visio</a>, <a href="http://www.microsoft.com/expression/products/Design_Overview.aspx">Expression Design</a>. Sketch PDF. CSS code. The 960.css file is 5.4 KB. View <a href="http://bitbucket.org/nathansmith/960-grid-system/">repository</a>.</p><!-- end #description.grid_4.pull_4 --><hr /><div class="grid_6">    <p>        <a href="http://www.spry-soft.com/grids/"><img src="img/tool_css.gif" alt="Custom CSS generator" width="460" height="60" /></a>    </p></div><!-- end .grid_6 --><div class="grid_6">    <p>        <a href="http://gridder.andreehansson.se/"><img src="img/tool_bookmark.gif" alt="Grid overlay bookmark" width="460" height="60" /></a>    </p></div><!-- end .grid_6 -->

      **5：container\_xx与grid\_xx**

    /*这是该系统最基本最重要的用法，其中xx代表列数*//*用法：*/<div class="container_12">    <div id="sidebar" class="grid_2">sidebar</div>    <div id="content" class="grid_10">        <div id="main_content" class="grid_6 alpha">main content</div>        <div id="photos" class="grid_2">photo’s</div>        <div id="advertisements" class="grid_2 omega">advertisement</div>        <div class="clear"></div>    </div>    <div class="clear"></div></div>

以上转载自[IT北瓜](http://imleeo.com/)

**链接地址: **[http://imleeo.com/special-series/960-grid-system-introduction.html](http://imleeo.com/special-series/960-grid-system-introduction.html)

 

 

首先，你需要学习关于”如何让框架工作”。你可以通过自己的尝试来学习，不过我仍然会在这里为大家进行讲解，那就开始吧。

**不要编辑960.css**

先说一点需要注意的:不要编辑960.css文件，如果你修改了它，那么你今后将无法更新这个框架。

**读取网格**

在我们使用外部文件中的CSS代码之前，首先要在我们的HTML文件中调用它们。像这样调用:

> \<link rel=”stylesheet” type=”text/css” media=”all” href=”path/to/960/reset.css” /\>
> \<link rel=”stylesheet” type=”text/css” media=”all” href=”path/to/960/960.css” /\>
> \<link rel=”stylesheet” type=”text/css” media=”all” href=”path/to/960/text.css” /\>

当我们调用好它们以后，我们要调用自己的CSS文件了。例如，你也许会将你的CSS文件命名为style.css或site.css或者其它什么的。这样调用它:

> \<link rel=”stylesheet” type=”text/css” media=”all” href=”path/to/style.css” /\>

### Containers(容器)

在960框架中，你可以选择两种类名为.container\_12 和 .container\_16的容器。这两种容器都是960px的宽度(这就是为什么叫做960 grid),但他们的不同之处是它们包含不同数量的列。顾名思义，.container\_12的容器被分为12列，而 .container\_16被分为16列。这两种960px宽的容器都是水平居中的。

### Grids (网格)/ Columns(列)

你可以选择很多种不同的列宽组合，不过在这两种容器中是有所不同的。你可以通过打开960.css来了解这些宽度，但这对于设计一个网站并没有什么必要。在这里暴风彬彬将一个很有用的技巧让你使用框架更加容易。

例如:如果你想在你的容器中仅使用两列(分别是主内容区/侧边栏)，你可以这样做:

> \<div class=”container\_12″\>
> \<div class=”grid\_4″\>sidebar\</div\>
> \<div class=”grid\_8″\>main content\</div\>
> \</div\>

看到上面的代码你也许已经明白，不过我还是要讲一下。也就是说你在container\_**12**这个容器中使用了grid\_**4**和grid\_**8**两列，4+8恰好等于12！明白了吗？使用网格系统的好处之一就是你不用去计算没列的宽度到底是多少，省去了很多运算。

下面让我们看看如何编写四列布局:

> \<div class=”container\_12″\>
> \<div class=”grid\_2″\>sidebar\</div\>
> \<div class=”grid\_6″\>main content\</div\>
> \<div class=”grid\_2″\>photo’s\</div\>
> \<div class=”grid\_2″\>advertisement\</div\>
> \</div\>

正如你看到的，这个系统工作得很好。如果你尝试使用你的浏览器读取他的话，你会发现有一些不对劲的地方。不过不要紧，那正是我们下一个话题要讨论的。

### Margins

默认情况下，每列之间都会存在一些margin。每个grid\_(这里插入数值)类都有10px的左margin和右margin。也就是说两列之间的margin值是20px。

20px的margin能让布局保持应有的留白并看上去更平滑，这也是我喜欢960 grid System的原因之一。

在上面的例子中，我们将它使用浏览器读取时出现了一些问题，现在我们来修复它。

问题在于每个列都包含左margin和右margin，但是最左面的列不应该有左margin，最右面的列不应该有右margin。(够罗嗦！)下面是解决方法:

> \<div class=”container\_12″\>
> \<div class=”grid\_2 alpha”\>sidebar\</div\>
> \<div class=”grid\_6″\>main content\</div\>
> \<div class=”grid\_2″\>photo’s\</div\>
> \<div class=”grid\_2 omega”\>advertisement\</div\>
> \</div\>

你仅需添加*alpha*类来去除左margin，添加*omega*类去除右margin。好了，现在我们的布局已经可以完美在浏览器中对齐了。(是的，包括IE6)

### Styling(添加样式)

事实上，你已经掌握了如何使用960框架创建基本的网格布局了。不过你也许还想为自己的布局添加一些样式。

> \<div class=”container\_12″\>
> \<div id=”sidebar” class=”grid\_2 alpha”\>sidebar\</div\>
> \<div id=”content” class=”grid\_6″\>main content\</div\>
> \<div id=”photos” class=”grid\_2″\>photo’s\</div\>
> \<div id=”advertisements” class=”grid\_2 omega”\>advertisement\</div\>
> \</div\>

由于CSS使用优先级的形式来觉得如何解释样式，而id要比class的优先级高。这样我们就可以在我们的独立CSS文件中以id选择符创建个性化的样式了。如果凑巧有的样式属性与960相同但值又不同，浏览器会优先选择你的[CSS](http://blog.bingo929.com/category/technology/css)文件中的样式。当然，如果您感兴趣，也可以[看看上面的实例添加样式后的实际效果](http://www.divitodesign.com/dd-articles/960-css-framework-basics/)。

英文原文:[960 CSS Framework – Learn the Basics](http://www.divitodesign.com/2008/12/960-css-framework-learn-basics/ "960 CSS Framework - Learn the Basics")

 

本框架代码适用于所有由yahoo评为A级（A-grade）的浏览器，yahoo对浏览器的评定情况如下图所示。 

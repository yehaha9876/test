[Blueprint CSS Framework 进阶](http://www.cnblogs.com/fdszlzl/archive/2009/06/10/1500361.html)
==============================================================================================

CSS Frameworks + CSS Reset: Design From Scratch

http://www.smashingmagazine.com/2007/09/21/css-frameworks-css-reset-design-from-scratch

**介绍**

blueprint是一个所谓的css framework，相比较而言blueprint 代码中的注释还是比较详细的。

按照Jeff Croft的Frameworks for Designers(或中文版本 理解Web框架，和如何构建一个CSS框架)描述的如何构建一个css framework的方法：

**blueprint的构建方式也与此类似**：

**分而治之**：

buleprint在功能组织上，将诸如布局(layout)、排版(typography)、组件（widget）、重置（reset）、打印 (print)等功能分散到不同的css文件中。这样方便使用者只需要导入自己所要使用的功能，不用导入全部文件，提高页面装载性能。目前在组件部分只提 供了对button的处理，尚未做到麦肯锡的MECE（"相互独立，完全穷尽"）的道。

**统一接口**：

尽管功能分散到多个css文件，但是导入时候，仍然只需要包含同样的文件screen.css文件，具体的导入细节在screen.css中再处理，统一了对外接口。

\<link rel="stylesheet" href="css/blueprint/screen.css" type="text/css" media="screen, projection" /\>

blueprint 所包含的css文件说明：

screen.css

This is the main file of the framework. It imports other CSS files from the "lib" directory, and should be included on every page.

类似于Jeff Croft的base.css功能，只需要包含此文件，就可以导入

print.css

This file sets some default print rules, so that printed versions included on every page.

用于处理打印，可以归类为widget。

lib/grid.css

This file sets up the grid (it's true). It has a lot of classes you apply to divs to set up any sort of column-based grid.

用于处理页面的布局（栏目）

lib/typography.css
This file sets some default typography. It also has a few methods for some really fancy stuff to do with your text.

用于处理页面元素的排版。

lib/reset.css

This file resets CSS values that browsers tend to set for you.

用于重置页面，对没有指定css属性的页面元素指定缺省值。

lib/buttons.css
Provides some great CSS-only buttons.

用于处理按钮,可以归类为widget

lib/compressed.css

A compressed version of the core files. Use this on every live site.

See screen.css for instructions

提供压缩过的（包含grid.css,tyopgraphy.css,reset.css,buttons.css）的css文件。

**2、各模块用法研究**

**2.1、grid.css研究**

对跨浏览器居中处理的兼容性处理

一般而言，要处理firefox、ie在处理居中时候的不同，采用如下方式：

body{text-align: center;}div\#container{margin-left: auto;margin-right: auto;width: 50em;text-align: left;}         

摘自：http://www.maxdesign.com.au/presentation/center/

**blueprint的处理方式：**

body { text-align: center; /\* IE6 Fix \*/ margin:36px 0;}

/\* A container should group all your columns. \*/

.container { text-align: left; position: relative; padding: 0; margin: 0 auto; /\* Centers layout \*/ width: 1150px; /\* Total width \*/ }

分栏（Columns）的实现     

blueprint的处理方式：

blueprint定义了.column , .span-x ,.last ，两者结合来实现分栏功能。

其中：.column定义栏目的float属性；.span-x定义栏目宽度；.last将margin-right置为0px,

<table>
<col width="100%" />
<tbody>
<tr class="odd">
<td align="left"><p>.column { float : left; margin-right: 10px; padding: 0;}</p>
<p>/* Use these classes to set how wide a column should be. */</p>
<p>.span-1 { width: 30px; }</p>
<p>.span-2 { width: 70px; }</p>
<p>.span-3 { width: 110px; }</p>
<p>.span-4 { width: 150px; }</p>
<p>....span-8 { width: 310px; }</p>
<p>.span-9 { width: 350px; }</p>
<p>.span-10 { width: 390px; }</p>
<p>....span-23 { width: 910px; }</p>
<p>.span-24 { width: 950px; margin: 0; }</p>
<p>.span-25 { width: 200px; }</p>
<p>.span-26 { width: 1150px; margin: 0; }</p>
<p>/* The last element in a multi-column block needs this class. */</p>
<p> .last { margin-right: 0; }</p>
<p>三栏的实现：</p>
<p>&lt;div class =&quot;container&quot; &gt;</p>
<p>&lt;div class =&quot;column span-24&quot; &gt; Header &lt;/div&gt;</p>
<p>&lt;div class =&quot;column span-4&quot; &gt; Left sidebar &lt;/div&gt;</p>
<p>&lt;div class =&quot;column span-16&quot; &gt; Main content &lt;/div&gt;</p>
<p>&lt;div class =&quot;column span-4 last&quot; &gt; Right sidebar &lt;/div&gt;</p>
<p>&lt;/div&gt;</p>
<p>四栏的实现：</p>
<p>&lt;div class =&quot;container&quot; &gt;</p>
<p>&lt;div class =&quot;column span-26&quot; &gt; Header &lt;/div&gt;</p>
<p>&lt;div class =&quot;column span-4&quot; &gt; Left sidebar &lt;/div&gt;</p>
<p>&lt;div class =&quot;column span-3 &quot; &gt; Main content 0 &lt;/div&gt;</p>
<p>&lt;div class =&quot;column span-25&quot; &gt; Main content 1 &lt;/div&gt;</p>
<p>&lt;div class =&quot;column span-4 last&quot; &gt; Right sidebar &lt;/div&gt;</p>
<p>&lt;/div&gt;</p>
<p>注意把.container中的width(定义了整个页面的width)修改为1150px</p>
<p>空白栏目的实现：对于多栏目（例如5栏目），其中有空白栏目（例如左右两栏目为空白），可以使用.append-x和.prepend-x来填充。</p>
<p>其中.append-x在栏目后（padding-right）添加空白栏目，.prepend-x在栏目前（padding-left）添加空白栏目。</p>
<p>通用的容器定义</p>
<p>/* A container should group all your columns. */</p>
<p> .container { text-align: left; position: relative; padding: 0; margin: 0 auto; /* Centers layout */ width: 1150px; /* Total width */ }</p></td>
</tr>
</tbody>
</table>

2.2、reset.css研究

reset.css的最初代码应该来自于Eric Meyer,Eric Meyer有两篇关于reset的日志，用于处理跨浏览器缺省值不同的问题。Eric Meyer的文档写得很精彩：

Reset Reasoning：http://meyerweb.com/eric/thoughts/2007/04/18/reset-reasoning/

Reset Reloaded：http://meyerweb.com/eric/thoughts/2007/05/01/reset-reloaded/

为何要reset
The basic reason is that all browsers have presentation defaults, but no browsers have the same defaults. (Okay, no two browser families—most Gecko-based browsers do have the same defaults.)
For example, some browsers indent unordered and ordered lists with left margins, whereas others use left padding. In past years, we tackled these inconsistencies on a case-by-case basis;

为何使用reset style，而不是universal selector

所谓universal selector 就是使用\* 来代表document所有的元素，例如

\* { margin: 0;} 关于reset style话题的一些资源：

YUI的reset库：http://developer.yahoo.com/yui/reset/

http://leftjustified.net/journal/2004/10/19/global-ws-reset/

以下几篇实际上是讨论css framework或tips的文章，只不过都提到了reset话题。

http://www.smashingmagazine.com/2007/05/10/70-expert-ideas-for-better-css-coding/

http://www.christianmontoya.com/2007/02/01/css-techniques-i-use-all-the-time/

http://businesslogs.com/design\_and\_usability/my\_5\_css\_tips.php

http://www.pingmag.jp/2006/05/18/5-steps-to-css-heaven/

**2.3、typography.css研究**

typography.css用于确定页面元素缺省的排版格式（baseline）：

Setting Type on the Web to a Baseline Grid：

http://alistapart.com/articles/settingtypeontheweb

**2.4、buttons.css研究**

Rediscovering the Button Element 讨论了用button元素来替代input元素的种种好处，button元素是提供了较为丰富的功能。

http://particletree.com/features/rediscovering-the-button-element

**2.4、print.css研究**

Eric Meyer有一篇关于css实现print功能的文章，可以作为理解print.css的参考。

CSS Design: Going to Print

Print Different

**2.5、compressed.css**

compressed.css是对blueprint几个文件压缩合成包，同时也对css文件进行了压缩，去除了无用的空格、换行、注释等文本。

此种方式用于在上生产系统部署时候使用，避免在页面导入多个css文件，同时也有利于提高页面性能。

按照lib/screen.css中的说明，应该使用了teenage提供的css压缩服务：

http://teenage.cz/acidofil/tools/cssformat.php

另外类似的提供对css、javascrīpt进行优化、压缩的服务还有：

http://csstidy.sourceforge.net/ （开源）

http://www.cssdrive.com/index.php/main/csscompressor/

http://www.cleancss.com/ （基于csstidy）

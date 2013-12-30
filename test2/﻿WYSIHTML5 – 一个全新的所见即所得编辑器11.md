wysihtml5 – 一个全新的所见即所得编辑器
======================================

[10六 2012](http://newhtml.net/wysihtml5-%e4%b8%80%e4%b8%aa%e5%85%a8%e6%96%b0%e7%9a%84%e6%89%80%e8%a7%81%e5%8d%b3%e6%89%80%e5%be%97%e7%bc%96%e8%be%91%e5%99%a8/ "2012 年 6 月 10 日 14:52")

by [liuyanghejerry](http://newhtml.net/author/admin/ "by liuyanghejerry") ⋅

如果你需要一个轻量级的HTML所见即所得编辑器，你或许可以考虑[wysihtml5](http://xing.github.com/wysihtml5/)了。

wysihtml5拥有许多独特之处，比如自动生成URL链接、不使用嵌入CSS等等，同时兼容支持包括IE8在内的众多浏览器，而在IE7、IE6等极为老旧的浏览器上可以仅仅显示为普通的编辑框来实现平缓降级。

如何使用
--------

### 包含脚本

包含wysihtml5所需脚本如下：

~~~~ {.wp-code-highlight .prettyprint}
<!-- wysihtml5 parser rules -->
<script src="/path-to-wysihtml5/parser_rules/advanced.js"></script>
<!-- Library -->
<script src="/path-to-wysihtml5/dist/wysihtml5-0.3.0.min.js"></script>
~~~~

### 创建编辑区域

~~~~ {.wp-code-highlight .prettyprint}
<form><textarea id="wysihtml5-textarea" placeholder="Enter your text ..." autofocus></textarea></form>
~~~~

这里的编辑框就是wysihtml5的目标编辑框，wysihtml5会将它转化为一个富文本编辑区域。

### 创建工具栏

~~~~ {.wp-code-highlight .prettyprint}
<div id="wysihtml5-toolbar" style="display: none;">
  <a data-wysihtml5-command="bold">bold</a>
  <a data-wysihtml5-command="italic">italic</a>
  
  <!-- Some wysihtml5 commands require extra parameters -->
  <a data-wysihtml5-command="foreColor" data-wysihtml5-command-value="red">red</a>
  <a data-wysihtml5-command="foreColor" data-wysihtml5-command-value="green">green</a>
  <a data-wysihtml5-command="foreColor" data-wysihtml5-command-value="blue">blue</a>
  
  <!-- Some wysihtml5 commands like 'createLink' require extra paramaters specified by the user (eg. href) -->
  <a data-wysihtml5-command="createLink">insert link</a>
  <div data-wysihtml5-dialog="createLink" style="display: none;">
    <label>
      Link:
      <input data-wysihtml5-dialog-field="href" value="http://" class="text">
    </label>
    <a data-wysihtml5-dialog-action="save">OK</a> <a data-wysihtml5-dialog-action="cancel">Cancel</a>
  </div>
</div>
~~~~

这一步创建编辑器所需的工具栏，以便加入一些常用的命令，比如加粗、斜体等等。

### 初始化wysihtml5

现在，终于可以初始化wysihtml5了：

~~~~ {.wp-code-highlight .prettyprint}
<script>
var editor = new wysihtml5.Editor("wysihtml5-textarea", { // id of textarea element
  toolbar:      "wysihtml5-toolbar", // id of toolbar element
  parserRules:  wysihtml5ParserRules // defined in parser rules set 
});
</script>
~~~~

### 重新定义CSS

这一步是可选的，但最好一起加上。原因是wysihtml5在将编辑框内容渲染时，没有采用内联CSS这一方式，比如给一段文字加粗时，wysihtml5并不会直接设置这段文字的CSS，而是为这段CSS加上一个预定义的class，通过class所关联的CSS来进行渲染。

因此，为了避免浏览器默认样式对此的影响，wysihtml5提供了一组CSS，可在初始化时如下配置：

~~~~ {.wp-code-highlight .prettyprint}
stylesheets: ["css/reset.css", "css/editor.css"]
~~~~

许可证
------

wysihtml5使用MIT协议发行。

分享到： 新浪微博 腾讯微博 人人网 EverNote [更多](http://www.jiathis.com/share?uid=1554190)

### 您可能还感兴趣：

[](http://newhtml.net/%e4%b8%93%e4%b8%ba%e7%a7%bb%e5%8a%a8%e5%b9%b3%e5%8f%b0%e6%89%93%e9%80%a0%e7%9a%84-jquery-jqmobi/)

专为移动平台打造的 jQuery - jqMobi

[](http://newhtml.net/6%e6%ac%be%e7%bf%bb%e4%b9%a6%e6%95%88%e6%9e%9cjquery%e6%8f%92%e4%bb%b6%e6%8e%a8%e8%8d%90/)

6款翻书效果jQuery插件推荐

[](http://newhtml.net/25%e4%b8%aa-html5%e4%b8%8ecss3%e7%bd%91%e7%ab%99%e8%ae%be%e8%ae%a1%e5%85%b8%e8%8c%83/)

25+个 HTML5与CSS3网站设计典范

Posted in: [HTML5](http://newhtml.net/category/html5/ "查看HTML5中的全部文章"), [JavaScirpt](http://newhtml.net/category/javascirpt/ "查看JavaScirpt中的全部文章"), [快速分享](http://newhtml.net/category/%e5%bf%ab%e9%80%9f%e5%88%86%e4%ba%ab/ "查看快速分享中的全部文章"), [资源](http://newhtml.net/category/%e8%b5%84%e6%ba%90/ "查看资源中的全部文章") ⋅ Tagged: [HTML5](http://newhtml.net/tag/html5/), [javascript](http://newhtml.net/tag/javascript/), [wysihtml5](http://newhtml.net/tag/wysihtml5/), [免费](http://newhtml.net/tag/%e5%85%8d%e8%b4%b9/), [创意](http://newhtml.net/tag/%e5%88%9b%e6%84%8f/), [工具](http://newhtml.net/tag/%e5%b7%a5%e5%85%b7/), [所见即所得](http://newhtml.net/tag/%e6%89%80%e8%a7%81%e5%8d%b3%e6%89%80%e5%be%97/), [资源](http://newhtml.net/tag/%e8%b5%84%e6%ba%90/)

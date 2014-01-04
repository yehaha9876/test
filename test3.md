ASP基础教程：ASP内建对象Request
===============================

[出处: http://book.jz123.cn/ASP/310.htm](http://book.jz123.cn/ASP/310.htm)

从本篇开始作者从 ASP 内建对象着手，为大家详细剖析 ASP 的六个内建对象和各种组件的特性和方法。

在正式开始学习 ASP 的内建对象和组件之前，先让我们来认识一些基本概念，这将对各位今后的学习大有帮助。请看下表：



这里的变量 db 就是 ASP 程序创建的访问数据库的对象实例。

Active Server Pages 提供了可在脚本中使用的内建对象。这些对象使用户更容易收集通过浏览器请求发送的信息、响应浏览器以及存储用户信息 , 从而使对象开发者摆脱了很多烦琐的工作。目前的 ASP 版本总共提供了六个内建对象，下面让我们将通过实例分别来进行学习。

**一、Request 对象**

可以使用 Request 对象访问任何基于 HTTP 请求传递的所有信息，包括从 HTML 表格用 POST 方法或 GET 方法传递的参数、cookie 和用户认证。Request 对象使您能够访问客户端发送给服务器的二进制数据。

**Request 的语法 :**

**Request[. 集合 | 属性 | 方法 ]( 变量 )**

在这里作者将挑选一些常用的对象语法进行分析

**1、Form**

Form 集合通过使用 POST 方法的表格检索邮送到 HTTP 请求正文中的表格元素的值。

**语法**

|Request.Form(element)[(index)|.Count]|

**参数**

element 指定集合要检索的表格元素的名称。

index 可选参数，使用该参数可以访问某参数中多个值中的一个。它可以是 1 到 Request.Form(parameter).Count 之间的任意整数。

**Count 集合中元素的个数**

Form 集合按请求正文中参数的名称来索引。Request.Form(element) 的值是请求正文中所有 element 值的数组。通过调用 Request.Form(element).Count 来确定参数中值的个数。如果参数未关联多个值，则计数为 1。如果找不到参数，计数为 0。要引用有多个值的表格元素中的单个值，必须指定 index 值。index 参数可以是从 1 到 Request.Form(element).Count 中的任意数字。如果引用多个表格参数中的一个，而未指定 index 值，返回的数据将是以逗号分隔的字符串。

可以使用重述符来显示表格请求中的所有数据值。例如，用户通过指定几个值填写表格，见下图。

![](../upload/20091123133824734.gif)

对于 hobby 参数，您可以使用下面的脚本检索这些值。

||
|＜ html＞ 
＜ head＞＜ title＞＜ /title＞＜ /head＞ ＜ body＞ 
＜ p＞ 请填写你的爱好 ＜ /p＞ 
＜ form method="POST" action="form.asp"＞ 
＜ p＞＜ input type="text" name="hobby" size="20"＞＜ br＞ 
＜ input type="checkbox" name="hobby" value=" 足球 "＞ 足球 ＜ input type="checkbox" name="hobby" value=" 乒乓球 "＞ 乒乓球 ＜ /p＞ 
＜ p＞＜ input type="submit" value=" 发送 " name="B1"＞＜ input type="reset" value=" 重填 " name="B2"＞＜ /p＞ 
＜ /form＞ 
＜ % For Each i In Request.Form("hobby") Response.Write i & "＜ BR＞" Next %＞ 
＜ /body＞＜ /html＞|

将以上代码剪贴到记事簿中（注意将“＜ ”后面的空格去掉），保存为 form.asp 文件并运行，request 对象可以根据你在 form 中填入或选择元素内容的不同将元素逐个显示出来。

当然使用 For...Next 循环也可以生成同样的输出，如下所示 :

||
|＜ % 
For i = 1 To Request.Form("hobby").Count\< 
Response.Write Request.Form("hobby")(i) & "＜ BR＞"Next\< 
%＞|

**2、QueryString**

QueryString 集合检索 HTTP 查询字符串中变量的值 ,HTTP 查询字符串由问号 (?) 后的值指定。如：

||
|＜ A HREF= "example.asp?string=this is a sample"＞string sample＜ /A＞|

生成值为 "this is a sample" 的变量名字符串。通过发送表格或由用户在其浏览器的地址框中键入查询也可以生成查询字符串。

**语法**

||
|Request.QueryString(variable)[(index)|.Count]|

QueryString 集合可以让您以名称检索 QUERY\_STRING 变量。Request.QueryString( 参数 ) 的值是出现在 QUERY\_STRING 中所有参数的值的数组。通过调用Request.QueryString(parameter).Count 可以确定参数有多少个值。

我们也可以使用 QueryString 来达到与前一个范例相同的功能。只需要将 request.form 部分替换如下：

||
|＜ % 
For Each i In Request.querystring("hobby") 
Response.Write i & "＜ BR＞" 
Next 
%＞|

3、Cookies

什么是 Cookie?Cookie 其实是一个标签，当你访问一个需要唯一标识你的站址的 WEB 站点时，它会在你的硬盘上留下一个标记，下一次你访问同一个站点时，站点的页面会查找这个标记。每个 WEB 站点都有自己的标记，标记的内容可以随时读取，但只能由该站点的页面完成。每个站点的 Cookie 与其他所有站点的 Cookie 存在同一文件夹中的不同文件内（你可以在 Windows 的目录下的 Cookie 文件夹中找到它们）。一个 Cookie 就是一个唯一标识客户的标记，Cookie 可以包含在一个对话期或几个对话期之间某个 WEB 站点的所有页面共享的信息，使用 Cookie 还可以在页面之间交换信息。Request 提供的 Cookies 集合允许用户检索在 HTTP 请求中发送的 cookie 的值。这项功能经常被使用在要求认证客户密码以及电子公告板、WEB 聊天室等 ASP 程序中。

**语法**

||
|Request.Cookies(cookie)[(key)|.attribute]|

**参数**

cookie 指定要检索其值的 cookie。

key 可选参数，用于从 cookie 字典中检索子关键字的值。

attribe 指定 cookie 自身的有关信息。如：HasKeys 只读，指定 cookie 是否包含关键字。

可以通过包含一个 key 值来访问 cookie 字典的子关键字。如果访问 cookie 字典时未指定 key，则所有关键字都会作为单个查询字符串返回。例如，如果 MyCookie 有两个关键字 , First 和 Second，而在调用 Request.Cookies 时并未指定其中任何一个关键字，那么将返回下列字符串。

||
|First=firstkeyvalue&Second=secondkeyvalue |

如果客户端浏览器发送了两个同名的 cookie，那么 Request.Cookie 将返回其中路径结构较深的一个。例如，如果有两个同名的的 cookie，但其中一个的路径属性为 /www/ 而另一个为 /www/home/，客户端浏览器同时将两个 cookie 都发送到 /www/home/ 目录中，那么 Request.Cookie 将只返回第二个 cookie。

要确定某个 cookie 是不是 cookie 字典（cookie 有否有关键字），可使用下列脚本。

||
|＜ %= Request.Cookies("myCookie").HasKeys %＞|

如果 myCookie 是一个 cookie 字典，则前面的赋值为 TRUE。否则，为 FALSE。下面我们来看看一个 cookie 的应用实例：

||
|＜ % 
nickname=request.form("nick")response.cookies("nick")=nickname 
\\' 用 response 对象将用户名写入 Cookie 之中 
response.write " 欢迎 "&request.cookies("nick")&" 光临小站！" 
%＞ 
＜ html＞＜ head＞＜ meta http-equiv="Content-Type" content="text/html; charset=gb2312"＞ 
＜ title＞cookie＜ /title＞ 
＜ meta name="GENERATOR" content="Microsoft FrontPage 3.0"＞＜ /head＞ 
＜ body＞ 
＜ form method="POST" action="cookie.asp"＞ 
＜ p＞＜ input type="text" name="nick" size="20"＞ 
＜ input type="submit" value=" 发送 " name="B1"＞＜ input type="reset" value=" 重填 " name="B2"＞＜ /p＞＜ /form＞ 
＜ /body＞＜ /html＞|

这其实是一个在基于 WEB 的 BBS 或 CHAT 的 ASP 程序中常用的手法，它将用户在起始页面上填入的姓名保存在 cookie 中，这样后面的程序就可以很容易地调用该用户的 nick 了。

**4、ServerVariables**

大家都知道在浏览器中浏览网页的时候使用的传输协议是 HTTP，在 HTTP 的标题文件中会记录一些客户端的信息，如 : 客户的 IP 地址等等，有时服务器端需要根据不同的客户端信息做出不同的反映，这时候就需要用 ServerVariables 集合获取所需信息。

**语法**

||
|Request.ServerVariables ( 服务器环境变量 )|

由于服务器环境变量较多，作者仅将一些常用的变量在下表中列出 :

||
|ALL\_HTTP|客户端发送的所有 HTTP 标题文件。|
|CONTENT\_LENGTH|客户端发出内容的长度。|
|CONTENT\_TYPE|内容的数据类型。如：“text/html”。同附加信息 的查询一起使用，如 HTTP 查询 GET、POST 和 PUT。|
|LOCAL\_ADDR|返回接受请求的服务器地址。如果在绑定多 个 IP 地址的多宿主机器上查找请求所使用的地址 时，这条变量非常重要。|
|LOGON\_USER|用户登录 Windows NT 的帐号。|
|QUERY\_STRING|查询 HTTP 请求中问号（?）后的信息。|
|REMOTE\_ADDR|发出请求的远程主机 (client) 的 IP 地址。|
|REMOTE\_HOST|发出请求的主机 (client) 名称。如果服务器无此 信息，它将设置为空的　MOTE\_ADDR 变量。|
|REQUEST\_METHOD|该方法用于提出请求。相当于用于 HTTP 的 GET、HEAD、POST等 等。|
|SERVER\_NAME|出现在自引用 URL 中的服务器主机名、DNS 化名 或 IP 地址。|
|SERVER\_PORT|发送请求的端口号。|

我们可以使用以下脚本打印出所有的服务器环境变量。

||
|＜ TABLE＞ 
＜ TR＞＜ TD＞＜ B＞Server Variable＜ /B＞＜ /TD＞＜ TD＞＜ B＞Value＜ /B＞＜ /TD＞＜ /TR＞＜ % For Each name In Request.ServerVariables %＞ 
＜ TR＞＜ TD＞ ＜ %= name %＞ ＜ /TD＞＜ TD＞ ＜ %= Request.ServerVariables(name) %＞ ＜ /TD＞＜ /TR＞＜ /TABLE＞ 
＜ % Next %＞|

今天我们详细学习了 ASP 内建对象中的 request 对象，这也是 ASP 程序中使用最频繁的对象，希望大家在课后多多实践。



[出处: http://blog.csdn.net/sd\_tz\_wzg/article/details/17381431](http://blog.csdn.net/sd_tz_wzg/article/details/17381431)

### [JSP 内置对象---request](http://blog.csdn.net/sd_tz_wzg/article/details/17381431)

分类： [java](http://blog.csdn.net/wzg775192833/article/category/1696967) [JSP](http://blog.csdn.net/wzg775192833/article/category/1796721) 2013-12-17 18:23 237人阅读 [收藏](javascript:void(0); "收藏") [举报](#report "举报")

[jsp](http://www.csdn.net/tag/jsp)[request](http://www.csdn.net/tag/request)[内置对象](http://www.csdn.net/tag/%e5%86%85%e7%bd%ae%e5%af%b9%e8%b1%a1)[乱码](http://www.csdn.net/tag/%e4%b9%b1%e7%a0%81)[请求协议](http://www.csdn.net/tag/%e8%af%b7%e6%b1%82%e5%8d%8f%e8%ae%ae)

JSP 内置对象
  有些对象不用声明就可以在JSP 页面的脚本部分使用，这就是JSP的内置对象。
  JSP 的内置对象有：resquest 、response、session、 application 、out
  response 和request 对象是JSP 的内置对象较重要的两个，这两个对象提供了对服务器和浏览器通信方法的控制。
 1.HTTP 协议----Word Wide Web 底层协议
  被称作“请求和响应”协议
 2.request 对象
  HTTP 通信协议是客户与服务器之间一种提交（请求）信息与响应信息（request/respone）的通信协议。
  在JSP 中，内置对象request封装了用户提交的信息，那么该对象调用相应的方法可以获取封装的信息，即使用该对象可以获取用户提交的信息。
  2.1Get 方法和post方法的主要区别是：
   使用get 方法提交的信息会在提交的过程中显示在浏览器的地址栏中，而post 方法提交的信息不会显示在地址栏中。
  2.2获取客户提交的信息：
   request 对象获取客户提交信息的最常用的方法是getParameter(String s)。s为标签的name属性。
  2.3处理汉字信息：
   当用request 对象获取客户提交的汉字字符时，会出现乱码问题，所以对含有汉字字符的信息必须进行特殊的处理方式。首先，将获取
  的字符串用ISO-8859-1 进行编码，并将编码存放到一个字节数组中，然后再将这个数组转化为字符串对象即可。
   String str=request.getParameter("girl");
   byte b[]=str.getBytes(“ISO-8859-1”);
   str=new String(b);
  2.4JSP 引擎的内置对象request 对象来获取客户提交的信息
   2.4.1. getProtocol() 获取客户向服务器提交信息所使用的通信协议，比如http/1.1 等。
   2.4.2. getServletPath() 获取客户请求的JSP 页面文件的目录。
   2.4.3. getContentLength() 获取客户提交的整个信息的长度。
   2.4.4. getMethod() 获取客户提交信息的方式，比如：post 或get.
   2.4.5. getHeader(String s) 获取HTTP 头文件中由参数s 指定的头名字的值,一般来说s 参数可取的头名有：accept、 referer、
   accept-language 、content-type 、 accept-encoding 、user-agent、host、 content-length、 connection、cookie 等，
   比如，s 取值user-agent 将获取客户的浏览器的版本号等信息。
   2.4.6. getHeaderNames() 获取头名字的一个枚举
   2.4.7. getHeaders(String s) 获取头文件中指定头名字的全部值的一个枚举
   2.4.8. getRemoteAddr() 获取客户的IP 地址。
   2.4.9. getRemoteHost() 获取客户机的名称（如果获取不到，就获取IP 地址）。
   2.4.10. getServerName() 获取服务器的名称。
   2.4.11. getServerPort() 获取服务器的端口号。
   2.4.12. getParameterNames() 获取客户提交的信息体部分中name参数值的一个枚举。

更多

上一篇：[JSP元素和标签](http://blog.csdn.net/sd_tz_wzg/article/details/17249811)

下一篇：[JSP内置对象----response](http://blog.csdn.net/sd_tz_wzg/article/details/17403903)

顶  
1

踩  
0

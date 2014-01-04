[出处: http://blog.csdn.net/sd\_tz\_wzg/article/details/17381431](http://blog.csdn.net/sd_tz_wzg/article/details/17381431)

### [JSP 内置对象---request](http://blog.csdn.net/sd_tz_wzg/article/details/17381431)

分类： [java](http://blog.csdn.net/wzg775192833/article/category/1696967) [JSP](http://blog.csdn.net/wzg775192833/article/category/1796721) 2013-12-17 18:23 237人阅读 [收藏](javascript:void(0); "收藏") [举报](#report "举报")

[jsp](http://www.csdn.net/tag/jsp)[request](http://www.csdn.net/tag/request)[内置对象](http://www.csdn.net/tag/%e5%86%85%e7%bd%ae%e5%af%b9%e8%b1%a1)[乱码](http://www.csdn.net/tag/%e4%b9%b1%e7%a0%81)[请求协议](http://www.csdn.net/tag/%e8%af%b7%e6%b1%82%e5%8d%8f%e8%ae%ae)

JSP 内置对象

  有些对象不用声明就可以在JSP 页面的脚本部分使用，这就是JSP的内置对象。
  
  JSP 的内置对象有：resquest 、response、session、 application 、out
  
  response 和request 对象是JSP 的内置对象较重要的两个，这两个对象提供了对服务器和浏览器通信方法的控制。
 1.HTTP 协议----Word Wide Web 底层协议

  
JSP 内置对象  
  有些对象不用声明就可以在JSP 页面的脚本部分使用，这就是JSP的内置对象。  
  JSP 的内置对象有：resquest 、response、session、 application 、out  
  response 和request 对象是JSP 的内置对象较重要的两个，这两个对象提供了对服务器和浏览器通信方法的控制。  
 1.HTTP 协议----Word Wide Web 底层协议  
  被称作“请求和响应”协议  
 2.request 对象  
  HTTP 通信协议是客户与服务器之间一种提交（请求）信息与响应信息（request/respone）的通信协议。  
  在JSP 中，内置对象request封装了用户提交的信息，那么该对象调用相应的方法可以获取封装的信息，即使用该对象可以获取用户提交的信息。  

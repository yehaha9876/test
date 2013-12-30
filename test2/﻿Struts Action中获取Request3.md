[Struts Action中获取Request](/u011628171/article/details/17459555)
==================================================================

共四种方式，示例在下面给出，其中第二种常用。

【注意1】：我们需要知道前两种方法得到的是Map\<String,Object\>,而后两种方式得到的才是真正的request等对象。而Map就是把request对象中的属性取出做成了键值对而已。

【注意2】：另外如果就是为了在action和[jsp](http://cpro.baidu.com/cpro/ui/uijs.php?rs=1&u=http%3A%2F%2Fblog%2Eknowsky%2Ecom%2F220083%2Ehtm&p=baidu&c=news&n=10&t=tpclicked3_hc&q=sayyescpr&k=jsp&k0=jsp&sid=6e0d24cd950a1ec5&ch=0&tu=u1366390&jk=26ddec34c7c445d2&cf=1&fv=11&stid=9&urlid=0)传递参数的话，只需要在action中定义成员，然后Jsp中利用struts标签\<s:property value="name"/\>就能够访问到数据，而这些内容都是被保存在了value stack中。关于value stack 和 stack context 会在后面得内容涉及。

?

**方法一：**

public class LoginAction1 extends ActionSupport {
 private Map request;
 private Map session;
 private Map application;
 public LoginAction1() {
 request = (Map)ActionContext.getContext().get("request");
 session = ActionContext.getContext().getSession();
 application = ActionContext.getContext().getApplication();
}
 public String execute() {
 request.put("r1", "r1");
 session.put("s1", "s1");
 application.put("a1", "a1");
 return SUCCESS;
 }
 }

**方法二：**

public class LoginAction2 extends ActionSupport implements RequestAware,SessionAware, ApplicationAware {
 private Map\<String, Object\> request;
 private Map\<String, Object\> session;
 private Map\<String, Object\> application;
 //DI dependency injection
 //IoC inverse of control
 public String execute() {
 request.put("r1", "r1");
 session.put("s1", "s1");
 application.put("a1", "a1");
 return SUCCESS;
 }

?  @Override
 public void setRequest(Map\<String, Object\> request) {
 this.request = request;
 }

@Override
 public void setSession(Map\<String, Object\> session) {
 this.session = session;
 }

@Override
 public void setApplication(Map\<String, Object\> application) {
 this.application = application;
 }
}

**方法三：**

public class LoginAction3 extends ActionSupport {
 private HttpServletRequest request;
 private HttpSession session;
 private ServletContext application;
 public LoginAction3() {
 request = ServletActionContext.getRequest();
 session = request.getSession();
 application = session.getServletContext();
}
 public String execute() {
 request.setAttribute("r1", "r1");
 session.setAttribute("s1", "s1");
 application.setAttribute("a1", "a1");
 return SUCCESS;
 }
 }

**方法四：**

public class LoginAction4 extends ActionSupport implements ServletRequestAware {
 private HttpServletRequest request;
 private HttpSession session;
 private ServletContext application;
 public String execute() {
 request.setAttribute("r1", "r1");
 session.setAttribute("s1", "s1");
 application.setAttribute("a1", "a1");
 return SUCCESS;
 }

 @Override
 public void setServletRequest(HttpServletRequest request) {
 this.request = request;
 this.session = request.getSession();
 this.application = session.getServletContext();
 }
}

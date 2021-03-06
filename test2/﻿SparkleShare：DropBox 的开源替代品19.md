[【网盘】SparkleShare：DropBox 的开源替代品](/xiaoxuan415315/article/details/8089787)
=====================================================================================

-   [![](/pictures/)目录视图](http://blog.csdn.net/xiaoxuan415315?viewmode=contents)
-   [![](/pictures/)摘要视图](http://blog.csdn.net/xiaoxuan415315?viewmode=list)

[投票赢好礼，周周有惊喜！](http://vote.blog.csdn.net/blogstar2013)      [2014年4月微软MVP申请开始了！](http://blog.csdn.net/blogdevteam/article/details/17440147)      [消灭0回答，赢下载分](http://blog.csdn.net/csdn_ask/article/details/16943685)      [“我的2013”年度征文活动火爆进行中！](http://blog.csdn.net/blogdevteam/article/details/17356319)      [专访Kinect手语翻译系统团队](http://www.csdn.net/article/2013-12-17/2817828)

### 

分类： [IOS\_数据存储](/xiaoxuan415315/article/category/1195845) 2012-10-19 14:38 513人阅读 [收藏](javascript:void(0); "收藏") [举报](#report "举报")

[服务器](http://www.csdn.net/tag/%e6%9c%8d%e5%8a%a1%e5%99%a8)[ubuntu](http://www.csdn.net/tag/ubuntu)[linux](http://www.csdn.net/tag/linux)[存储](http://www.csdn.net/tag/%e5%ad%98%e5%82%a8)[git](http://www.csdn.net/tag/git)[文档](http://www.csdn.net/tag/%e6%96%87%e6%a1%a3)

上周本来计划介绍的，无奈工作比较忙，日志在草稿箱里躺了几天。今天在 Buzz 上看到 [Wow!Ubuntu](http://wowubuntu.com/) 上[Riku](http://riku.wowubuntu.com/) 君已经发表了比较完整的介绍，因此我就不重复造轮子了，直接分享给大家。[原文地址](http://wowubuntu.com/sparkleshare.html)。 相信使用过 DropBox 的用户一定会对它喜爱有加，我也是 DropBox 的忠实用户。可现在的问题是，DropBox 由于各方面的原因在国内已经无法使用了。所以我很希望有一款能代替它的开源产品出现，很巧的是，我找到了 SparkleShare 。 正是因为上述原因，SparkleShare 项目在刚启动时我就对它特别的关注。从官方介绍来看，SparkleShare 就是一个 DropBox 的开源替代品，具备了 DropBox 应有的特性，包括同步、版本控制等等。它还有更多的优点，比如可以自建服务器、与 Gnome 友好集成、完全免费等等。[![](/pictures/ "zrtn_001n31b56868_tn")](http://www.syncoo.com/wp-content/uploads/2010/09/zrtn_001n31b56868_tn.jpg) 项目主页： [http://sparkleshare.org/](http://sparkleshare.org/) 另外，按照官方计划，SparkleShare 将会支持 Linux 、Win 及 Mac 平台，但目前只有 Linux 版本。虽然它还没有提供可用的二进制包，但已经是一个可用的测试版了，只要稍加编译就可使用，安装过程请看后面的介绍。 目前 SparkleShare 支持通过自建的 Git 服务器、Github、Gitorius 及 Gnome Project 来同步及存储文件，从以上这些服务的特性来看，严格意义上说 SparkleShare 相当于是一个源代码控制管理软件 Git 的前端程序。 \# 安装： 先安装依赖包：

> sudo apt-get install gtk-sharp2 monodevelop mono-devel libndesk-dbus1.0-cil-dev libndesk-dbus-glib1.0-cil-dev python-nautilus git-core intltool gvfs gvfs-bin python-gtk2-dev openssh-client

到[这里](http://www.bomahy.nl/hylke/blog/sparkleshare-02-beta-1-for-linux/)下载源代码：版本为 0.2 beta1 ( 最新的源代码可以从[这里](http://gitorious.org/sparkleshare)获取 ) 解压缩后编译安装：

> ./configure –prefix=/usr make sudo make install

启动：

> sparkleshare start

\# 使用 我这边是利用了 [Github](http://github.com/) 服务来进行存储及同步文件，所以你必须先去 Github 注册一个帐号并创建应用池。另外要提醒一下的是，在 Github 上创建的应用池都是开放的，所有人都可以看到你上传的文件，所以如果你有保密文件的话不建议用 Github ，建议自建服务器。 创建完应用池后，你还需要用 ssh-keygen 创建 ssh key ，然后上传到 Github 后台，详细的操作方法见[这里](http://help.github.com/msysgit-key-setup/)。上传完后你可以用以下命令进行验证。

> ssh [git@github.com](mailto:git@github.com)

出现 You’ve successfully authenticated 字样的话，说明 key 上传成功。如果出现以下错误

> Agent admitted failure to sign using the key.

你需进行 ssh-add 命令

> ssh-add \~/.ssh/id\_dsa

当验证成功后就可以设置 SparkleShare 进行同步了，如下图。[![](/pictures/ "2010_09_27_114706")](http://www.syncoo.com/wp-content/uploads/2010/09/2010_09_27_114706.png)

官方提供的[详细文档介绍](http://www.sparkleshare.org/documentation.php)。

@xdash：文末我再追加两张官方提供的截图：[![](/pictures/ "notification")](http://www.syncoo.com/wp-content/uploads/2010/09/notification.png) 托盘图标 [![](/pictures/ "statusicon")](http://www.syncoo.com/wp-content/uploads/2010/09/statusicon1.png) 右下角菜单 相关链接：[SparkleShare 官方项目主页](http://sparkleshare.org/) | [via Wow!Ubuntu](http://wowubuntu.com/sparkleshare.html) | [来自同步控](http://www.syncoo.com/sparkleshare-2-3676.htm)
查看原文：[http://www.syncoo.com/sparkle-share-3676.htm](http://www.syncoo.com/sparkle-share-3676.htm)

更多

上一篇：[关于 cocos2d-x 好的学习博客](/xiaoxuan415315/article/details/8089679)

下一篇：[苹果开发者帐号(Company)申请流程](/xiaoxuan415315/article/details/8089831)

\* 以上用户言论只代表其个人观点，不代表CSDN网站的观点或立场

[![TOP](/pictures/)](# "回到顶部")

##### [核心技术类目](http://www.csdn.net/tag/)

[全部主题](http://www.csdn.net/tag "全部主题") [Java](http://www.csdn.net/tag/Java "Java") [VPN](http://www.csdn.net/tag/vpn "VPN") [Android](http://www.csdn.net/tag/android "Android") [iOS](http://www.csdn.net/tag/ios "iOS") [ERP](http://www.csdn.net/tag/erp "ERP") [IE10](http://www.csdn.net/tag/ie10 "IE10") [Eclipse](http://www.csdn.net/tag/eclipse "Eclipse") [CRM](http://www.csdn.net/tag/crm "CRM") [JavaScript](http://www.csdn.net/tag/javascript "JavaScript") [Ubuntu](http://www.csdn.net/tag/ubuntu "Ubuntu") [NFC](http://www.csdn.net/tag/nfc "NFC") [WAP](http://www.csdn.net/tag/wap "WAP") [jQuery](http://www.csdn.net/tag/jquery "jQuery") [数据库](http://www.csdn.net/tag/数据库 "数据库") [BI](http://www.csdn.net/tag/bi "BI") [HTML5](http://www.csdn.net/tag/html5 "HTML5") [Spring](http://www.csdn.net/tag/spring "Spring") [Apache](http://www.csdn.net/tag/apache "Apache") [Hadoop](http://www.csdn.net/tag/hadoop "Hadoop") [.NET](http://www.csdn.net/tag/.net ".NET") [API](http://www.csdn.net/tag/api "API") [HTML](http://www.csdn.net/tag/html "HTML") [SDK](http://www.csdn.net/tag/sdk "SDK") [IIS](http://www.csdn.net/tag/iis "IIS") [Fedora](http://www.csdn.net/tag/fedora "Fedora") [XML](http://www.csdn.net/tag/xml "XML") [LBS](http://www.csdn.net/tag/lbs "LBS") [Unity](http://www.csdn.net/tag/unity "Unity") [Splashtop](http://www.csdn.net/tag/splashtop "Splashtop") [UML](http://www.csdn.net/tag/uml "UML") [components](http://www.csdn.net/tag/components "components") [Windows Mobile](http://www.csdn.net/tag/windowsmobile "Windows Mobile") [Rails](http://www.csdn.net/tag/rails "Rails") [QEMU](http://www.csdn.net/tag/qemu "QEMU") [KDE](http://www.csdn.net/tag/kde "KDE") [Cassandra](http://www.csdn.net/tag/cassandra "Cassandra") [CloudStack](http://www.csdn.net/tag/cloudstack "CloudStack") [FTC](http://www.csdn.net/tag/ftc "FTC") [coremail](http://www.csdn.net/tag/coremail "coremail") [OPhone](http://www.csdn.net/tag/ophone%20 "OPhone ") [CouchBase](http://www.csdn.net/tag/couchbase "CouchBase") [云计算](http://www.csdn.net/tag/云计算 "云计算") [iOS6](http://www.csdn.net/tag/iOS6 "iOS6") [Rackspace](http://www.csdn.net/tag/rackspace%20 "Rackspace ") [Web App](http://www.csdn.net/tag/webapp "Web App") [SpringSide](http://www.csdn.net/tag/springside "SpringSide") [Maemo](http://www.csdn.net/tag/maemo "Maemo") [Compuware](http://www.csdn.net/tag/compuware "Compuware") [大数据](http://www.csdn.net/tag/大数据 "大数据") [aptech](http://www.csdn.net/tag/aptech "aptech") [Perl](http://www.csdn.net/tag/perl "Perl") [Tornado](http://www.csdn.net/tag/tornado "Tornado") [Ruby](http://www.csdn.net/tag/ruby "Ruby") [Hibernate](http://www.csdn.net/hibernate "Hibernate") [ThinkPHP](http://www.csdn.net/tag/thinkphp "ThinkPHP") [Spark](http://www.csdn.net/tag/spark "Spark") [HBase](http://www.csdn.net/tag/hbase "HBase") [Pure](http://www.csdn.net/tag/pure "Pure") [Solr](http://www.csdn.net/tag/solr "Solr") [Angular](http://www.csdn.net/tag/angular "Angular") [Cloud Foundry](http://www.csdn.net/tag/cloudfoundry "Cloud Foundry") [Redis](http://www.csdn.net/tag/redis "Redis") [Scala](http://www.csdn.net/tag/scala "Scala") [Django](http://www.csdn.net/tag/django "Django") [Bootstrap](http://www.csdn.net/tag/bootstrap "Bootstrap")

[![](/pictures/ "访问我的空间")](http://my.csdn.net/xiaoxuan415315)
[xiaoxuan415315](http://my.csdn.net/xiaoxuan415315)

[](javascript:void(0); "[加关注]") [](javascript:void(0); "[发私信]")

-   访问：85119次
-   积分：1280分
-   排名：第9427名

-   原创：17篇
-   转载：160篇
-   译文：0篇
-   评论：31条

-   [iOS](http://blog.csdn.net/xiaoxuan415315/article/category/1181072)(39)
-   [IOS\_UITableView](http://blog.csdn.net/xiaoxuan415315/article/category/1184193)(9)
-   [IOS\_UIButton](http://blog.csdn.net/xiaoxuan415315/article/category/1195711)(1)
-   [IOS\_画图](http://blog.csdn.net/xiaoxuan415315/article/category/1195744)(6)
-   [IOS\_UIImage](http://blog.csdn.net/xiaoxuan415315/article/category/1195764)(3)
-   [IOS\_UIScrollView](http://blog.csdn.net/xiaoxuan415315/article/category/1195822)(3)
-   [IOS\_手势](http://blog.csdn.net/xiaoxuan415315/article/category/1195832)(3)
-   [IOS\_数据存储](http://blog.csdn.net/xiaoxuan415315/article/category/1195845)(15)
-   [IOS\_HTML](http://blog.csdn.net/xiaoxuan415315/article/category/1195996)(8)
-   [IOS\_UISearchBar](http://blog.csdn.net/xiaoxuan415315/article/category/1204923)(4)
-   [IOS \_动画](http://blog.csdn.net/xiaoxuan415315/article/category/1204933)(1)
-   [IOS\_MAP](http://blog.csdn.net/xiaoxuan415315/article/category/1208134)(8)
-   [IOS\_HTTP](http://blog.csdn.net/xiaoxuan415315/article/category/1208273)(6)
-   [IOS\_语法](http://blog.csdn.net/xiaoxuan415315/article/category/1214511)(13)
-   [IOS\_NSString](http://blog.csdn.net/xiaoxuan415315/article/category/1230927)(2)
-   [IOS\_TextField](http://blog.csdn.net/xiaoxuan415315/article/category/1234520)(3)
-   [IOS\_广告](http://blog.csdn.net/xiaoxuan415315/article/category/1239650)(6)
-   [其他](http://blog.csdn.net/xiaoxuan415315/article/category/1244780)(1)
-   [面试题](http://blog.csdn.net/xiaoxuan415315/article/category/1261053)(3)
-   [IOS\_XCODE](http://blog.csdn.net/xiaoxuan415315/article/category/1261055)(18)
-   [IOS\_COCOS2D-X](http://blog.csdn.net/xiaoxuan415315/article/category/1261063)(1)
-   [IOS\_多线程](http://blog.csdn.net/xiaoxuan415315/article/category/1272416)(4)
-   [IOS\_NSNotification](http://blog.csdn.net/xiaoxuan415315/article/category/1273679)(4)
-   [IOS\_框架（生命周期）](http://blog.csdn.net/xiaoxuan415315/article/category/1286244)(7)
-   [IOS\_错误信息](http://blog.csdn.net/xiaoxuan415315/article/category/1289850)(9)
-   [IOS\_XML](http://blog.csdn.net/xiaoxuan415315/article/category/1312409)(2)
-   [IOS\_推送](http://blog.csdn.net/xiaoxuan415315/article/category/1312530)(2)
-   [IOS\_UIPickerView](http://blog.csdn.net/xiaoxuan415315/article/category/1350674)(2)
-   [IOS\_SVN](http://blog.csdn.net/xiaoxuan415315/article/category/1358360)(3)
-   [IOS\_UIWebView](http://blog.csdn.net/xiaoxuan415315/article/category/1481659)(2)

-   [2013年12月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/12)(4)
-   [2013年11月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/11)(2)
-   [2013年07月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/07)(3)
-   [2013年06月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/06)(3)
-   [2013年05月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/05)(3)
-   [2013年04月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/04)(4)
-   [2013年03月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/03)(6)
-   [2013年02月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/02)(3)
-   [2013年01月](http://blog.csdn.net/xiaoxuan415315/article/month/2013/01)(6)
-   [2012年12月](http://blog.csdn.net/xiaoxuan415315/article/month/2012/12)(20)
-   [2012年11月](http://blog.csdn.net/xiaoxuan415315/article/month/2012/11)(31)
-   [2012年10月](http://blog.csdn.net/xiaoxuan415315/article/month/2012/10)(17)
-   [2012年09月](http://blog.csdn.net/xiaoxuan415315/article/month/2012/09)(14)
-   [2012年08月](http://blog.csdn.net/xiaoxuan415315/article/month/2012/08)(41)
-   [2012年07月](http://blog.csdn.net/xiaoxuan415315/article/month/2012/07)(20)

-   [Ios 程序打包,安装流程](/xiaoxuan415315/article/details/8191109 "Ios 程序打包,安装流程")(4434)
-   [警告 incomplete implementation](/xiaoxuan415315/article/details/8433892 "警告 incomplete implementation")(2562)
-   [UIButton 设置圆角 边框颜色 点击回调方法](/xiaoxuan415315/article/details/7787683 "UIButton 设置圆角 边框颜色 点击回调方法")(2422)
-   [将gb2312转化成utf-8重新解析](/xiaoxuan415315/article/details/7896846 "将gb2312转化成utf-8重新解析")(2309)
-   [面试题 （一）](/xiaoxuan415315/article/details/8061666 "面试题 （一）")(2291)
-   [Apple Mach-O Linker Error错误](/xiaoxuan415315/article/details/8314017 "Apple Mach-O Linker Error错误")(2220)
-   [发布iOS应用程序(Application Loader)](/xiaoxuan415315/article/details/8217134 "发布iOS应用程序(Application Loader)")(1987)
-   [Google Maps JavaScript API V2 服务](/xiaoxuan415315/article/details/7848195 "Google Maps JavaScript API V2 服务")(1942)
-   [ASIHTTPRequest iphone下post和get数据的经典类库 配置](/xiaoxuan415315/article/details/7848370 "ASIHTTPRequest   iphone下post和get数据的经典类库 配置")(1637)
-   [上传app](/xiaoxuan415315/article/details/8216697 "上传app")(1504)

-   [Ios 程序打包,安装流程](/xiaoxuan415315/article/details/8191109 "Ios 程序打包,安装流程")(3)
-   [在IOS应用中从竖屏模式强制转换为横屏模式](/xiaoxuan415315/article/details/8000885 "在IOS应用中从竖屏模式强制转换为横屏模式")(2)
-   [【网盘】SparkleShare：DropBox 的开源替代品](/xiaoxuan415315/article/details/8089787 "【网盘】SparkleShare：DropBox 的开源替代品")(2)
-   [UITableView 背景透明](/xiaoxuan415315/article/details/7737525 "UITableView 背景透明")(2)
-   [下拉刷新原理](/xiaoxuan415315/article/details/8467948 "下拉刷新原理")(2)
-   [ios layer的一些学习](/xiaoxuan415315/article/details/8208298 "ios layer的一些学习")(2)
-   [iOS: 用libxml2 and hpple来做html parser](/xiaoxuan415315/article/details/7848227 "iOS: 用libxml2 and hpple来做html parser")(1)
-   [tableview下拉刷新](/xiaoxuan415315/article/details/8467930 "tableview下拉刷新")(1)
-   [iPhone 移植到 iPad（一）](/xiaoxuan415315/article/details/8136834 "iPhone 移植到 iPad（一）")(1)
-   [xocde4.3 如何手动安装iPhone simulator 4.3/5.0](/xiaoxuan415315/article/details/7843409 "xocde4.3 如何手动安装iPhone simulator 4.3/5.0")(1)



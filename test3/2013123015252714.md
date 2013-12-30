[使用RestSharp 库调用Restful Service](/nnsword/article/details/7719239)
=======================================================================

现在互联网上的服务接口都是Restful的，SOAP的Service已经不是主流。.NET/Mono下如何调用Restful Service呢，再也没有了方便的Visual Studio的方便生产代理的工具了，你还在用HttpWebRequest 自己封装吗？Restful Service还有授权问题，自己写出来的代码是不是很不优雅？通常Restful Service返回的数据格式是XML或者Json，还要设置服务的输入参数等等，使用起来很复杂。本文向你推荐一个开源的库RestSharp轻松消费Restful Service。[http://restsharp.org/](http://restsharp.org/)是一个开源的.NET平台下REST和Http API的客户端库，支持的平台有.NET 3.5/4、Mono、Mono for Android、MonoTouch、Windows Phone 7.1 Mango。他可以简化我们访问Restful服务，可以到这里下载代码[https://github.com/johnsheehan/RestSharp/archives/master](https://github.com/johnsheehan/RestSharp/archives/master) 更简单的使用[NuGet](http://nuget.org/List/Packages/RestSharp)。RestSharp使用Json.Net处理 Json数据同Poco对象的序列化。

下面分别从库的使用方式上进行介绍，使用的Restful Service是腾讯社区开放平台（[http://opensns.qq.com/](http://opensns.qq.com/)）。
 1、服务认证，RestSharp定义了一个认证授权的接口 IAuthenticator ，有NtlmAuthenticator、HttpBasicAuthenticator、OAuth1Authenticator、OAuth2Authenticator几种，基本上可以满足要求了，腾讯社区开放平台使用OAuth2，腾讯社区开放平台额外增加了一个OpenId的参数，我们从OAuth2Authenticator的基类继承实现一个：

    public class OAuthUriQueryParameterAuthenticator : OAuth2Authenticator
     {
         private readonly string openId;
         private readonly string consumerKey;

        public OAuthUriQueryParameterAuthenticator(string openId, string accessToken, string consumerkey)
             :base(accessToken)
         {
             this.openId = openId;
             this.consumerKey = consumerkey;
         }

        public override void Authenticate(IRestClient client, IRestRequest request)
         {
             request.AddParameter("access\_token", AccessToken, ParameterType.GetOrPost);
             request.AddParameter("openid", openId, ParameterType.GetOrPost);
             request.AddParameter("oauth\_consumer\_key", consumerKey, ParameterType.GetOrPost);
         }

2、Get请求方法，下面的例子是根据access\_token获得对应用户身份的openid： [https://graph.qq.com/oauth2.0/me?access\_token=YOUR\_ACCESS\_TOKEN](https://graph.qq.com/oauth2.0/me?access_token=YOUR_ACCESS_TOKEN)

     public string GetOpenId(string accessToken)
       {

          RestClient  \_restClient = new RestClient(Endpoints.ApiBaseUrl);
           var request = \_requestHelper.CreateOpenIDRequest(accessToken);
           var response = Execute(request);
           var openid = GetUserOpenId(response.Content);
           return openid;
       }

       private RestSharp.RestResponse Execute(RestRequest request)
        {

       //返回的结果

           var response = \_restClient.Execute(request);

           if (response.StatusCode != HttpStatusCode.OK)
            {
                throw new QzoneException(response);
            }
            return response;
        }

       internal RestRequest CreateOpenIDRequest(string accesstoken)
        {
            var request = new RestRequest(Method.GET);
            request.Resource = "oauth2.0/me?access\_token={accesstoken}";
            request.AddParameter("accesstoken", accesstoken, ParameterType.UrlSegment);
            return request;
        }

      上面代码里涉及到了服务的输入参数通过AddParameter方法很方便的处理，是不是很简单。

3、POST请求服务，下面的例子是发表一条微博信息（纯文本）到腾讯微博平台上[http://wiki.opensns.qq.com/wiki/%E3%80%90QQ%E7%99%BB%E5%BD%95%E3%80%91add\_t](http://wiki.opensns.qq.com/wiki/%E3%80%90QQ%E7%99%BB%E5%BD%95%E3%80%91add_t "http://wiki.opensns.qq.com/wiki/%E3%80%90QQ%E7%99%BB%E5%BD%95%E3%80%91add_t")：

        /// \<summary\>
         /// 发表一条微博信息（纯文本）到腾讯微博平台上
         /// \</summary\>
         /// \<param name="content"\>表示要发表的微博内容。必须为UTF-8编码，最长为140个汉字，也就是420字节。
         /// 如果微博内容中有URL，后台会自动将该URL转换为短URL，每个URL折算成11个字节。\</param\>
         /// \<param name="clientip"\>用户ip，以分析用户所在地\</param\>
         /// \<param name="jing"\>用户所在地理位置的经度。为实数，最多支持10位有效数字。有效范围：-180.0到+180.0，+表示东经，默认为0.0\</param\>
         /// \<param name="wei"\>用户所在地理位置的纬度。为实数，最多支持10位有效数字。有效范围：-90.0到+90.0，+表示北纬，默认为0.0。\</param\>
         /// \<param name="syncflag"\>标识是否将发布的微博同步到QQ空间（0：同步； 1：不同步；），默认为0.\</param\>
         /// \<returns\>\</returns\>
         internal AddWeiboResult AddWeibo(string content, string clientip = "", string jing = "", string wei = "", int syncflag = 0)
         {

        RestClient  \_restClient = new RestClient(Endpoints.ApiBaseUrl);

             \_restClient.Authenticator = new OAuthUriQueryParameterAuthenticator(context.AccessToken.OpenId, context.AccessToken.AccessToken, context.Config.GetAppKey());
             var request = \_requestHelper.CreateAddWeiboRequest(content, clientip,jing,wei,syncflag);

            var response = Execute(request);           
             var payload = Deserialize\<AddWeiboResult\>(response.Content);
             return payload;
         }

       internal RestRequest CreateAddWeiboRequest(string content, string clientip, string jing, string wei, int syncflag)
         {
             var request = new RestRequest(Method.POST);
             request.RequestFormat = DataFormat.Json;
             request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
             request.Resource = "t/add\_t";
             request.AddParameter("content", content);
             if (!string.IsNullOrEmpty(clientip))
             {
                 request.AddParameter("clientip", clientip);
             }
             if (!string.IsNullOrEmpty(jing))
             {
                 request.AddParameter("jing", jing);
             }
             if (!string.IsNullOrEmpty(wei))
             {
                 request.AddParameter("wei", wei);
             }
             request.AddParameter("syncflag", syncflag);
             return request;
         }

   这个方法需要使用到OAuth2的认证和前面的不需要认证的接口比较起来并没有变复杂，代码很优雅。

4、来点复杂的，发个图片微博，RestSharp对HttpFile的封装也很不错，使用起来一样很简单，看代码中的红色部分：

internal RestRequest CreateAddPictureWeiboRequest(string content, string clientip, string jing, string wei, int syncflag, string fileName, byte[] bytes)
        {
            var request = new RestRequest(Method.POST);
            request.RequestFormat = DataFormat.Json;
            var boundary = string.Concat("--", Util.GenerateRndNonce());
            request.AddHeader("Content-Type", string.Concat("multipart/form-data; boundary=", boundary));
            request.Resource = "t/add\_pic\_t";
            request.AddParameter("content", content);
            if (!string.IsNullOrEmpty(clientip))
            {
                request.AddParameter("clientip", clientip);
            }
            if (!string.IsNullOrEmpty(jing))
            {
                request.AddParameter("jing", jing);
            }
            if (!string.IsNullOrEmpty(wei))
            {
                request.AddParameter("wei", wei);
            }
            request.AddParameter("syncflag", syncflag);
            **request.AddFile("pic", bytes, fileName);
**           return request;
        }

上面这几个API的调用已经很具有代表性了，是不是可以很好的简化你使用Restful Service，记住DRY(don’t repeat yourself)，可以很好的加速你的应用的开发。
